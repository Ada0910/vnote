# 1. 核心概念
# 2. State 

　　vuex的状态管理，需要依赖它的状态树，官网说：

```
　　Vuex 使用单一状态树——是的，用一个对象就包含了全部的应用层级状态。至此它便作为一个“唯一数据源 (SSOT)”而存在。这也意味着，每个应用将仅仅包含一个 store 实例。单一状态树让我们能够直接地定位任一特定的状态片段，在调试的过程中也能轻易地取得整个当前应用状态的快照
```

**我们要把我们需要做状态管理的量放到这里来，然后在后面的操作动它**
　　我们来声明一个state：
```
const state = { 
 blogTitle: '迩伶贰blog',
 views: 10,
 blogNumber: 100,
 total: 0,
 todos: [
 {id: 1, done: true, text: '我是码农'},
 {id: 2, done: false, text: '我是码农202号'},
 {id: 3, done: true, text: '我是码农202号'}
 ]
}
```
## 2.1. 在 Vue 组件中获得 Vuex 状态
- 由于 Vuex 的状态存储是响应式的，从 store 实例中读取状态最简单的方法就是在计算属性中返回某个状态：
```
// 创建一个 Counter 组件
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return store.state.count
    }
  }
}
//每当 store.state.count 变化的时候, 都会重新求取计算属性，并且触发更新相关联的 DOM
```
Vuex 通过 store 选项，提供了一种机制将状态从根组件“注入”到每一个子组件中（需调用 Vue.use(Vuex)）：
```
const app = new Vue({
  el: '#app',
  // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
  store,
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
})
```
通过在根实例中注册 store 选项，该 store 实例会注入到根组件下的所有子组件中，且子组件能通过 this.$store 访问到。让我们更新下 Counter 组件 的实现：
```
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```



# 3. Mutation

　我们有了state状态树，我们要改变它的状态（值），就必须用vue指定唯一方法 mutation，官网说：

　更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的 事件类型 (type) 和 一个 回调函数 (handler)。

　简单粗暴理解：任何不以mutation的方式改变state的值，都是耍流氓（违法）

   我们来一个mutation：

```
const mutation = {
 addViews (state) {
 state.views++
 },
 blogAdd (state) {
 state.blogNumber++
 },
 clickTotal (state) {
 state.total++
 }
}
```
# 4. Action

　　action 的作用跟mutation的作用是一致的，它提交mutation，从而改变state，是改变state的一个增强版，官网说：

　　Action 类似于 mutation，不同在于：

Action 提交的是 mutation，而不是直接变更状态。

Action 可以包含任意异步操作。

　　简单粗暴理解： 额，这总结的差不多了，就这样理解吧！

　　我们来一个action：

```
const actions = {
 addViews ({commit}) {
 commit('addViews')
 },
 clickTotal ({commit}) {
 commit('clickTotal')
 },
 blogAdd ({commit}) {
 commit('blogAdd')
 }
}
```
# 5. Getter

官网说：有时候我们需要从 store 中的 state 中派生出一些状态，例如对列表进行过滤并计数。减少我们对这些状态数据的操作

简单粗暴理解：状态树上的数据比较复杂了，我们使用的时候要简化操作，上面的state.todos 是一个对象，在组件中挑不同的数据时，需要对其做下处理，这样每次需要一次就处理一次，我们简化操作，将其在getter中处理好，然后export 一个方法；

 我们来一个getter：

```
const getters = {
 getToDo (state) {
 return state.todos.filter(item => item.done === true)
 // filter 迭代过滤器 将每个item的值 item.done == true 挑出来， 返回的是一个数组
 }
}
```

## 5.1. mapState 辅助函数
当一个组件需要获取多个状态时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 mapState 辅助函数帮助我们生成计算属性，让你少按几次键：
// 在单独构建的版本中辅助函数为 Vuex.mapState
```
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({
  
    // 箭头函数可使代码更简练
    count: state => state.count,

    // 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}
```
当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 mapState 传一个字符串数组。
```
computed: mapState([
  // 映射 this.count 为 store.state.count
  'count'
])
```
由于 mapState 函数返回的是一个对象，在ES6的写法中，我们可以通过对象展开运算符，可以极大的简化写法:
```
computed: {
  localComputed () { /* ... */ },
  
  // 使用对象展开运算符将此对象混入到外部对象中
  ...mapState({
    // ...
  })
}

//相当于将 state的属性，都添加到computed，而且指向state中的数据

```
