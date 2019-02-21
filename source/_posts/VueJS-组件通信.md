---
title: VueJS-组件通信
date: 2017-02-27
categories: React、Vue
---
前端在处理交互时，经常会遇到一个问题:组件之间如何通信？
下面介绍几种常用的组件通信方式,包括父子组件，兄弟组件，复杂组件下的不同通信方式

1. #### 现代通信方式: FLUX VUEX
  __FLUX将一个应用分成四个部分__:[文档](http://www.ruanyifeng.com/blog/2016/01/flux.html)
    1. View： 视图层
    1. Action（动作）：视图层发出的消息（比如mouseClick）
    1. Dispatcher（派发器）：用来接收Actions、执行回调函数
    1. Store（数据层Model）：用来存放应用的状态，一旦发生变动，就提醒Views要更新页面

  flux最大的特性是“单向数据流”，具体流程：
    1. 用户访问 View,
    1. View 发出用户的 Action,
    1. Dispatcher 收到 Action，要求 Store 进行相应的更新
    1. Store 更新后，发出一个"change"事件
    1. View 收到"change"事件后，更新页面
    <img src="/img/flux.png" width = "480" height = "200" align=center />

  __VUEX__:[官方文档](https://vuex.vuejs.org/zh-cn/intro.html)
    Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。
    我们把组件的共享状态抽取出来，以一个全局单例模式管理,在这种模式下，我们的组件树构成了一个巨大的“视图”，不管在树的哪个位置，任何组件都能获取状态或者触发行为！
    注：action可以是异步的，但mutations一定是同步的

    对于一个的VUEX应用来说,FLUX的概念被具化成：
    1. VUE: 视图层
    1. Action（动作）：视图层发出的消息（比如mouseClick）
    1. Mutations（派发器）：用来接收Actions、执行回调函数
    1. State（数据层Model）：用来存放应用的状态，一旦发生变动，就提醒Vue要更新页面
     <img src="/img/vuex.png" width = "650" height = "480" alt="vuex" align=center />

2. #### 父子组件通信:props，events
  __父->子__: 父组件使用 props 把数据传给子组件,文档如下
  [https://cn.vuejs.org/v2/guide/components.html#使用-Prop-传递数据](https://cn.vuejs.org/v2/guide/components.html#使用-Prop-传递数据)
  ```html
  <child message="hello!"></child>
  ```
  ```js
  Vue.component('child', {
    // 声明 props
    props: ['message'],
    // 就像 data 一样，prop 可以用在模板内
    // 同样也可以在 vm 实例中像 “this.message” 这样使用
    template: '<span>{{ message }}</span>'
  })
  ```
  需要__注意__的是，子组件不能修改父组件的props
  因为一个父组件下可能有多个子组件，如果某个子组件修改了父组件传递的props，
  很可能导致其他子组件也就跟着变化，最终导致整个应用的状态难以管理和维护
  所以不允许子组件修改props

  __子->父__: 子组件自定义事件，父组件可以在使用子组件的地方直接用 v-on 来监听子组件触发的事件
  ```html
  <div id="counter-event-example">
    <p>{{ total }}</p>
    <button-counter v-on:increment="incrementTotal"></button-counter>
    <button-counter v-on:increment="incrementTotal"></button-counter>
  </div>
  ```
  ```js
    Vue.component('button-counter', {
      template: '<button v-on:click="increment">{{ counter }}</button>',
      data: function () {
        return {
          counter: 0
        }
      },
      methods: {
        increment: function () {
          this.counter += 1
          this.$emit('increment')
        }
      },
    })

    new Vue({
      el: '#counter-event-example',
      data: {
        total: 0
      },
      methods: {
        incrementTotal: function () {
          this.total += 1
        }
      }
    })
  ```

3. #### 非父子组件通信: event bus
  官方文档：[https://cn.vuejs.org/v2/guide/components.html#非父子组件通信](https://cn.vuejs.org/v2/guide/components.html#非父子组件通信)
  有时候非父子关系的组件也需要通信。在简单的场景下，使用一个空的 Vue 实例作为中央事件总线
  在这两个组件之间引入这个中央事件总线，然后emit，on相应的事件
  ```js
  var bus = new Vue()

  // 触发组件 A 中的事件
  bus.$emit('id-selected', 1)

  // 在组件 B 创建的钩子中监听事件
  bus.$on('id-selected', function (id) {
    // ...
  })
  ```
4. #### 表单交互:双向绑定
  官方文档：[https://cn.vuejs.org/v2/guide/components.html#使用自定义事件的表单输入组件](https://cn.vuejs.org/v2/guide/components.html#使用自定义事件的表单输入组件)
  实际上VUE的双向绑定就是事件的一个语法糖
  ```html
  <input v-model="something">

  <input v-bind:value="something" v-on:input="something = $event.target.value">

  <custom-input v-bind:value="something" v-on:input="something = arguments[0]"></custom-input>
  ```
