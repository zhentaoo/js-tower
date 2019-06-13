### ES6+: 装饰器Decorator介绍

### 定义
1. 装饰器其实是一个对类进行处理的函数。装饰器函数的第一个参数，就是所要装饰的目标类。
2. ES6+里的装饰器，依赖 ES5 的 Object.defineProperty方法实现（vue的双向绑定也依赖这个方法）

### 代码举例

下面的代码中：
testable就是一个装饰器，并对Person类增加了一个属性
readonly也是一个装饰器，它对Person的属性进行readonly操作
```js

  function testable(target) {
    target.isTestable = 'aa'
  }

  // 对类属性进行装饰
  function readonly(target, name, descriptor) {
    descriptor.writable = false
    return descriptor
  }

  @testable
  class Person {
    @readonly
    name: 'xiao A'
  }

  console.log('xxx', Person.isTestable, 'xxx')

  setInterval(() => {
    console.log(Person.name, Person.isTestable, 1)
  }, 1000)

  setTimeout(() => {
    console.log('xiaob:')

    try {
      Person.name = 'xiao B'
    } catch (error) {
      console.log('xiaob:', error)
    }
  }, 5000)

```

注意：
装饰器只能用于类和类的方法，不能用于函数，因为存在函数提升。
如果一定要装饰函数，可以采用高阶函数的形式操作。