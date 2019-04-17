---
title: vue总结
date: 2018-01-17 15:52:54
tags: vue
categories: vue
---

从React转vue有半年时间，刚上手vue觉得很简单方便，不过越深入越发现vue这货比React要复杂，处处坑..

<!-- more -->


## 计算属性 vs Methods

计算属性是基于它们的依赖进行缓存的,计算属性只有在它的相关依赖发生改变时才会重新求值。这就意味着只要`message`还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数

## `v-show`支持

注意， v-show 不支持 `<template>` 语法，也不支持 v-else

## v-if 与 v-show

`v-if` 是“真正的”条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
`v-if` 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
相比之下， `v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。
一般来说， `v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件不太可能改变，则使用 `v-if` 较好。

## v-for 与 v-if的优先级

> 2.2.0+ 的版本里，当在组件中使用 v-for 时，key 现在是必须的。
当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级。

## 缩写

`<a v-bind:href="url"></a>` 缩写为 `<a :href="url"></a>` 属性可省略`v-bind`
`<a v-on:click="doSomething"></a>` 缩写为 `<a @click="doSomething"></a>` 监听DOM时间`v-on` 可转化为`@`

## 修饰符

修饰符（Modifiers）是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()：

```javascript
<form v-on:submit.prevent="onSubmit"></form>
```

之后当我们更深入地了解 v-on 与 v-model时，会看到更多修饰符的使用。

## v-for with v-if

当它们处于同一节点， v-for 的优先级比 v-if 更高，这意味着 v-if 将分别重复运行于每个 v-for 循环中。当你想为仅有的 一些 项渲染节点时，这种优先级的机制会十分有用，如下：

```javacript
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>
```

上面的代码只传递了未complete的todos。
而如果你的目的是有条件地跳过循环的执行，那么将 v-if 置于包装元素 (或 `<template>`)上。如:

```javascript
<ul v-if="shouldRenderTodos">
  <li v-for="todo in todos">
    {{ todo }}
  </li>
</ul>
```

## 数组更新

由于 JavaScript 的限制， Vue 不能检测以下变动的数组：
当你利用索引直接设置一个项时，例如： `vm.items[indexOfItem] = newValue` 可以用
 `Vue.set(example1.items, indexOfItem, newValue)`
 或者
 `example1.items.splice(indexOfItem, 1, newValue)`
当你修改数组的长度时，例如：`vm.items.length = newLength` 可以用
`example1.items.splice(newLength)`

## stopPropagation 与 preventDefault

`stopPropagation` 为阻止事件冒泡
`preventDefault` 为阻止的默认行为

## data 必须为函数

定义组件的时候data必须是一个函数
如：

```javascript
    Vue.component('simple-counter', {
        template: '<button v-on:click="counter += 1">{{ counter }}</button>',
        data: function () {
            return {
                counter: 0
            }
        }
    });
```
为神马呢？官方已经给出实例原因。因为组件可能被多处使用，但它们的data是私有的，所以每个组件都要return一个新的data对象，如果共享data，修改其中一个会影响其他组件。
## prop 转化问题

HTML 特性是不区分大小写的。所以，当使用的不是字符串模版，camelCased (驼峰式) 命名的 prop 需要转换为相对应的 kebab-case (短横线隔开式) 命名：

```javascript
    Vue.component('child', {
        // camelCase in JavaScript
        props: ['myMessage'],
        template: '<span>{{ myMessage }}</span>'
    });
    <!-- kebab-case in HTML -->
    <child my-message="hello!"></child>
```

## 单项数据流

prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是不会反过来。这是为了防止子组件无意修改了父组件的状态——这会让应用的数据流难以理解。

> 注意在 JavaScript 中对象和数组是引用类型，指向同一个内存空间，如果 prop 是一个对象或数组，在子组件内部改变它会影响父组件的状态。

## oninput 与 onchange 事件

`oninput` 是在 `<input>` 或 `<textarea>` 元素的值发生改变时触发, `onchange` 在元素失去焦点时触发

## vuex 状态管理示意图

![图片](https://vuex.vuejs.org/zh-cn/images/vuex.png)

## vue-apollo 与 vuex 结合一般有两种方法

第一种：
 apollo 这个对象里加一个属性 result，第一个参数是 graphql 返回的结果，然后手动存进 vuex
 ```javascript
 export default {
  data: () => ({
    posts: []
  }),

  apollo: {
    posts: {
      query: postsQuery,
      loadingKey: 'loading',
      result(data){
        this.$store.dispatch('get_posts', data)
      }
    }
  }
};
 ```

第二种方法：
vuex 里的获取函数直接写 Vue.prototype.$apollo.query(…)

## 用 v-on 监听原生事件注意点

在 Vue 2.0 中，为自定义组件绑定原生事件必须使用 .native 修饰符：

```javascript
<my-component @click.native="handleClick">Click Me</my-component>
```

从易用性的角度出发，`Element`对 Button 组件进行了处理，使它可以监听 click 事件：

```javascript
<el-button @click="handleButtonClick">Click Me</el-button>
```

是对于其他组件，还是需要添加 .native 修饰符。

## vue中不应该使用箭头函数的属性

- computed
- methods
- watcher

## 其他注意点

** 一个组件下只能有一个并列的 div **
Vue 不能检测到对象属性的添加或删除,由于 Vue 会在初始化实例时对属性执行 getter/setter 转化过程，所以属性必须在 data 对象上存在才能让 Vue 转换它，这样才能让它是响应的,Vue 不允许在已经创建的实例上动态添加新的根级响应式属性(root-level reactive property)。然而它可以使用 Vue.set(object, key, value) 方法将响应属性添加到嵌套的对象上：

```javascript
    Vue.set(vm.someObject, 'b', 2)
    或
    this.$set(this.someObject,'b',2)
```

有时你想向已有对象上添加一些属性，例如使用 Object.assign() 或 _.extend() 方法来添加属性。但是，添加到对象上的新属性不会触发更新。在这种情况下可以创建一个新的对象，让它包含原对象的属性和新的属性：

```javascript
// 代替 `Object.assign(this.someObject, { a: 1, b: 2 })`
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })
或
this.someObject = {...this.someObject, a: 1, b: 2}
```

** 箭头函数要注意 **
不要在实属性或者回调函数中（如 vm.$watch('a', newVal => this.myMethod())）使用箭头函数。因为箭头函数绑定父级上下文，所以 this 不会像预想的一样是 Vue 实例，而是 this.myMethod 未被定义。

## vue-apollo 与 vuex 结合一般有两种方法

第一种：
 apollo 这个对象里加一个属性 result，第一个参数是 graphql 返回的结果，然后手动存进 vuex
 ```javascript
 export default {
  data: () => ({
    posts: []
  }),

  apollo: {
    posts: {
      query: postsQuery,
      loadingKey: 'loading',
      result(data){
        this.$store.dispatch('get_posts', data)
      }
    }
  }
};
 ```

第二种方法：
vuex 里的获取函数直接写 Vue.prototype.$apollo.query(…)


---------

<center><img src="https://subscription-1255463026.cos.ap-guangzhou.myqcloud.com/subscription.png" width="180" ></center>