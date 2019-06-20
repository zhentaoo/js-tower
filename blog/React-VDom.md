## React核心概念-虚拟DOM

虚拟DOM大家一定听过很多遍了，vue、react、rn、weex都有，并且可以用它实现跨平台，但VDom具体是什么？
直接点说，其实Virtual Dom就是一个js对象.

### VDom的诞生
比如你在react中写了一个html片段
```js
<div id="app">
  <p class="color">hello world!!!</p>
</div>
```
再说一下babel，babel本身是个编译器结合jsx插件，经过语法分析+词法分析将上述html片段转化成AST抽象语法树，再将AST编译为js对象（这里就不详细说AST了，有兴趣的可以使用babel打印下AST就能大致了解）

```js
{
  tag: 'div',
  props: {
    id: 'app'
  },
  chidren: [
    {
      tag: 'p',
      props: {
        className: 'color'
      },
      chidren: [
        'hello world!!!'
      ]
    }
  ]
}
```
好了，这就是一个VDom对象

接下来，你可以在各个端，使用这个js对象+平台API，生成对应平台的UI了，这也是RN、Weex可以跨平台的根基。

### 渲染VDom
这里只介绍如何在浏览器中渲染VDom

```js
function render(vdom) {
  // 如果是字符串或者数字，创建一个文本节点
  if (typeof vdom === 'string' || typeof vdom === 'number') {
    return document.createTextNode(vdom)
  }
  const { tag, props, children } = vdom
  // 创建真实DOM
  const element = document.createElement(tag)
  // 设置属性
  setProps(element, props)
  // 遍历子节点，并获取创建真实DOM，插入到当前节点
  children
    .map(render)
    .forEach(element.appendChild.bind(element))

  // 虚拟 DOM 中缓存真实 DOM 节点
  vdom.dom = element
  
  // 返回 DOM 节点
  return element
}

function setProps (element, props) {
  Object.entries(props).forEach(([key, value]) => {
    setProp(element, key, value)
  })
}

function setProp (element, key, vlaue) {
  element.setAttribute(
    // className使用class代替
    key === 'className' ? 'class' : key,
    vlaue
  )
}
```
代码如上，在浏览器中，你可以使用document.createTextNode，document.createElement，element.setAttribute等api将一个js对象渲染完成。（IOS、android不再例举了）

### Diff算法
当你知道一个VDom是一个js对象后，那么diff其实就是对比新旧两个对象，做一次重新渲染。
对于diff算法性能，是有很多值得研究的细节。

具体可以研究一下snabbdom这个库的实现

未完待续...


