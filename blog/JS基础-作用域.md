---
title: 作用域
date: 2016-07-03
categories: JavaScript语言专题
---
### 定义
1. 作用域：决定了变量的可见性与生命周期
2. 作用域链：保证了作用域的有序访问

### 几个重要概念
- EC(执行环境或者执行上下文，Execution Context)
- ECS(执行环境栈Execution Context Stack)
- VO(变量对象，Variable Object)|AO(活动对象，Active Object)
- scope chain(作用域链)和[[scope]]属性

### EC(作用域／执行环境／执行上下文)
执行环境定义了变量或函数有权访问的其他数据

每当控制器到达ECMAScript可执行代码的时候，控制器就进入了一个执行上下文。

javascript中，EC分为三种：
- 全局级别的代码 – 这个是默认的代码运行环境，一旦代码被载入，引擎最先进入的就是这个环境。
- 函数级别的代码 – 当执行一个函数时，运行函数体中的代码。
- Eval的代码 – 在Eval函数内运行的代码。

EC建立分为两个阶段：进入执行上下文和执行阶段。
1. 进入上下文阶段：发生在函数调用时，但是在执行具体代码之前（比如，对函数参数进行具体化之前） 
2. 执行代码阶段：变量赋值，函数引用，执行其他代码。

我们可以将EC看做是一个对象。
```js
EC={
    VO:{/* 函数中的arguments对象, 参数, 内部的变量以及函数声明 */},
    this:{},
    Scope:{ /* VO以及所有父执行上下文中的VO */}
}
```

### ECS(执行环境栈Execution Context Stack)

一系列活动的执行上下文从逻辑上形成一个栈。
栈底总是全局上下文，栈顶是当前（活动的）执行上下文。
当在不同的执行上下文间切换（退出的而进入新的执行上下文）的时候，栈会被修改（通过压栈或者退栈的形式）。
压栈：全局EC-->局部EC1-->局部EC2-->当前EC 
出栈：全局EC<--局部EC1<--局部EC2<--当前EC

我们可以用数组的形式来表示环境栈:
ECS=[局部EC,全局EC];
每次控制器进入一个函数（哪怕该函数被递归调用或者作为构造器），都会发生压栈的操作。过程类似javascript数组的push和pop操作。

当javascript代码文件被浏览器载入后，默认最先进入的是一个全局的执行上下文。
当在全局上下文中调用执行一个函数时，程序流就进入该被调用函数内，此时引擎就会为该函数创建一个新的执行上下文，并且将其压入到执行上下文堆栈的顶部。
浏览器总是执行当前在堆栈顶部的上下文，一旦执行完毕，该上下文就会从堆栈顶部被弹出，然后，进入其下的上下文执行代码。这样，堆栈中的上下文就会被依次执行并且弹出堆栈，直到回到全局的上下文。

### VO|AO (变量对象，Variable Object | 活动对象，Active Object)

### VO

每一个EC都对应一个变量对象VO，在该EC中定义的所有变量和函数都存放在其对应的VO中。
VO分为全局上下文VO（全局对象，Global object，我们通常说的global对象）和函数上下文的AO。
```js
VO: {
  // 上下文中的数据 (变量声明（var）, 函数声明（FD), 函数形参（function arguments）)
}
```
1. 进入执行上下文时，VO的初始化过程具体如下：
  - 函数的形参（当进入函数执行上下文时） —— 变量对象的一个属性，其属性名就是形参的名字，其值就是实参的值；对于没有传递的参数，其值为undefined
  - 函数声明（FunctionDeclaration, FD） —— 变量对象的一个属性，其属性名和值都是函数对象创建出来的；如果变量对象已经包含了相同名字的属性，则替换它的值
  - 变量声明（var，VariableDeclaration） —— 变量对象的一个属性，其属性名即为变量名，其值为undefined;如果变量名和已经声明的函数名或者函数的参数名相同，则不会影响已经存在的属性。

  注意：该过程是有先后顺序的。
2. 执行代码阶段时，VO中的一些属性undefined值将会确定。

### AO

> 在函数的执行上下文中，VO是不能直接访问的。它主要扮演被称作活跃对象（activation object）（简称：AO）的角色。

这句话怎么理解呢，就是当EC环境为函数时，我们访问的是AO，而不是VO。

```js
VO(functionContext) === AO;
```

AO是在进入函数的执行上下文时创建的，并为该对象初始化一个arguments属性，该属性的值为Arguments对象。
```js

AO = {
  arguments: {
    callee:,
    length:,
    properties-indexes: //函数传参参数值
  }
};
```

FD的形式只能是如下这样：
```js
function f(){

}
```

### 示例
#### VO示例：
```js
alert(x); // function

var x = 10;
alert(x); // 10

x = 20;

function x() {};

alert(x); // 20
```

进入执行上下文时，
```js
ECObject={
  VO:{
    x:<reference to FunctionDeclaration "x">
  }
};
```
执行代码时：
```js
ECObject={
  VO:{
    x:20 //与函数x同名，替换掉，先是10，后变成20
  }
};
```

对于以上的过程，我们详细解释下。
在进入上下文的时候，VO会被填充函数声明； 同一阶段，还有变量声明“x”，但是，正如此前提到的，变量声明是在函数声明和函数形参之后，并且，变量声明不会对已经存在的同样名字的函数声明和函数形参发生冲突。因此，在进入上下文的阶段，VO填充为如下形式：

```js
VO = {};

VO['x'] = <引用了函数声明'x'>

// 发现var x = 10;
// 如果函数“x”还未定义
// 则 "x" 为undefined, 但是，在我们的例子中
// 变量声明并不会影响同名的函数值

VO['x'] = <值不受影响，仍是函数>
```

执行代码阶段，VO被修改如下：
```js
VO['x'] = 10;
VO['x'] = 20;
```

如下例子再次看到在进入上下文阶段，变量存储在VO中（因此，尽管else的代码块永远都不会执行到，而“b”却仍然在VO中）
```js
if (true) {
  var a = 1;
} else {
  var b = 2;
}

alert(a); // 1
alert(b); // undefined, but not "b is not defined"
```
#### AO示例：
```js
function test(a, b) {
  var c = 10;
  function d() {}
  var e = function _e() {};
  (function x() {});
}
test(10); // call
```

当进入test(10)的执行上下文时，它的AO为：
```js
testEC={
    AO:{
        arguments:{
            callee:test
            length:1,
            0:10
        },
        a:10,
        c:undefined,
        d:<reference to FunctionDeclaration "d">,
        e:undefined
    }
};
```

由此可见，在建立阶段，VO除了arguments，函数的声明，以及参数被赋予了具体的属性值，其它的变量属性默认的都是undefined。函数表达式不会对VO造成影响，因此，(function x() {})并不会存在于VO中。
当执行test(10)时，它的AO为：
```js
testEC={
    AO:{
        arguments:{
            callee:test,
            length:1,
            0:10
        },
        a:10,
        c:10,
        d:<reference to FunctionDeclaration "d">,
        e:<reference to FunctionDeclaration "e">
    }
};
```
可见，只有在这个阶段，变量属性才会被赋具体的值。

### 作用域链

在执行上下文的作用域中查找变量的过程被称为标识符解析(indentifier resolution)，这个过程的实现依赖于函数内部另一个同执行上下文相关联的对象——作用域链。作用域链是一个有序链表，其包含着用以告诉JavaScript解析器一个标识符到底关联着哪一个变量的对象。而每一个执行上下文都有其自己的作用域链Scope。

#### 一句话：作用域链Scope其实就是对执行上下文EC中的变量对象VO|AO有序访问的链表。能按顺序访问到VO|AO，就能访问到其中存放的变量和函数的定义。

Scope定义如下：
`Scope = AO|VO + [[Scope]]`

其中，AO始终在Scope的最前端，不然为啥叫活跃对象呢。即：

`Scope = [AO].concat([[Scope]]);`

这说明了，作用域链是在函数创建时就已经有了。
那么[[Scope]]是什么呢？
[[Scope]]是一个包含了所有上层变量对象的分层链，它属于当前函数上下文，并在函数创建的时候，保存在函数中。
`[[Scope]]是在函数创建的时候保存起来的——静态的（不变的），只有一次并且一直都存在——直到函数销毁。 比方说，哪怕函数永远都不能被调用到，[[Scope]]属性也已经保存在函数对象上了。`

```js
var x=10;
function f1(){
  var y=20;
  function f2(){
    return x+y;
  }
}
```
以上示例中，f2的[[scope]]属性可以表示如下：
```js
f2.[[scope]]=[
  f2OuterContext.VO
]
```

而f2的外部EC的所有上层变量对象包括了f1的活跃对象f1Context.AO，再往外层的EC，就是global对象了。
所以，具体我们可以表示如下：
```js
f2.[[scope]]=[
  f1Context.AO,
  globalContext.VO
]
```

对于EC执行环境是函数来说，那么它的Scope表示为：
`functionContext.Scope=functionContext.AO+function.[[scope]]`

注意，以上代码的表示，也体现了[[scope]]和Scope的差异，Scope是EC的属性，而[[scope]]则是函数的静态属性。
（由于AO|VO在进入执行上下文和执行代码阶段不同，所以，这里及以后Scope的表示，我们都默认为是执行代码阶段的Scope，而对于静态属性[[scope]]而言，则是在函数声明时就创建了）
对于以上的代码EC，我们可以给出其Scope的表示：
```js
exampelEC={
  Scope:[
    f2Context.AO+f2.[[scope]],
    f1.context.AO+f1.[[scope]],
    globalContext.VO
  ]
}
```
接下来，我们给出以上其它值的表示：
globalContext.VO
```js
globalContext.VO={
  x:10,
  f1:<reference to FunctionDeclaration "f1">
}
f2Context.AO
f2Context.AO={
  f1Context.AO={
    arguments:{
      callee:f1,
      length:0
    },
    y:20,
    f2:<reference to FunctionDeclaration "f2">
  }
}
```

f2.[[scope]]
```js
f2Context.AO={
  f1Context.AO:{
    arguments:{
      callee:f1,
      length:0
    },
    y:20,
    f2:<reference to FunctionDeclaration "f2">
  },
  globalContext.VO:{
    x:10,
    f1:<reference to FunctionDeclaration "f1">
  }
}
```

f1Context.AO
```js
f1Context.AO={
  arguments:{
    callee:f1,
    length:0
  },
  y:20,
  f2:<reference to FunctionDeclaration "f2">
}
```

f1.[scope]
```js
f1.[[scope]]={
  globalContext.VO:{
    x:undefined,
    f1:undefined
  }
}
```

好，我们知道，作用域链Scope呢，是用来有序访问VO|AO中的变量和函数，对于上面的示例，我们给出访问的过程：
x,f1
```js
- "x"
-- f2Context.AO // not found
-- f1Context.AO // not found
-- globalContext.VO // found - 10
```

f1的访问过程类似。
y
```js
- "y"
-- f2Context.AO // not found
-- f1Context.AO // found -20
```

总结

EC分为两个阶段，进入执行上下文和执行代码。
ECStack管理EC的压栈和出栈。
每个EC对应一个作用域链Scope，VO|AO（AO，VO只能有一个），this。
函数EC中的Scope在进入函数EC时创建，用来有序访问该EC对象AO中的变量和函数。
函数EC中的AO在进入函数EC时，确定了Arguments对象的属性；在执行函数EC时，其它变量属性具体化。
函数的[[scope]]属性在函数创建时就已经确定，并保持不变。


---


There are __5 key points__ to remember about the Execution Stack
1. Single threaded.
2. Synchronous execution.
3. Global context.
4. Infinite function contexts.
5. Each function call creates a new execution context, even a call to itself

Execution Context in Detail

So we now know that everytime a function is called, a new execution context is created.
However, inside the JavaScript interpreter, every call to an execution context has 2 stages:
- Creation Stage [when the function is called, but before it executes any code inside]:
- Create the Scope Chain.
- Create variables, functions and arguments.

- Determine the value of "this".
- Activation / Code Execution Stage:
- Assign values, references to functions and interpret / execute code.
- It is possible to represent each execution context conceptually as an object with 3 properties:
executionContextObj = {
    'scopeChain': { /* variableObject + all parent execution context's variableObject */ },
    'variableObject': { /* function arguments / parameters, inner variable and function declarations */ },
    'this': {}
}


Let’s look at an example:
```js
function foo(i) {
    var a = 'hello';
    var b = function privateB() {

    };
    function c() {

    }
}

foo(22);
On calling foo(22), the creation stage looks as follows:
fooExecutionContext = {
    scopeChain: { ... },
    variableObject: {
        arguments: {
            0: 22,
            length: 1
        },
        i: 22,
        c: pointer to function c()
        a: undefined,
        b: undefined
    },
    this: { ... }
}
```
