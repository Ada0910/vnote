# 1. 语法
```
每个 Vue 应用都需要通过实例化 Vue 来实现。
语法格式如下:
var vm = new Vue({
  // 选项
})
```
# 2. 指令
指令是带有 v- 前缀的特殊属性。
## 2.1. 常见的v指令
-  v-if
条件判断使用 v-if 指令

- v-for
循环

- 计算属性
computed

- v-model 
指令用来在 input、select、text、checkbox、radio 等表单控件元素上创建双向数据绑定，根据表单上的值，自动更新绑定的元素的值
按钮的事件我们可以使用 v-on 监听事件，并对用户的输入进行响应

-  v-on:click
它用于监听 DOM 事件：

-  v-bind
指令将该元素的 href 属性与表达式 url 的值绑定。
# 3. 缩写
## 3.1. v-bind 缩写
Vue.js 为两个最为常用的指令提供了特别的缩写：
```
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```
## 3.2. v-on 缩写
```
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>
<!-- 缩写 -->
<a @click="doSomething"></a>
```
# 4. 组件
## 4.1. 注册一个全局组件语法格式如下：

```
Vue.component(tagName, options)
```
tagName 为组件名，options 为配置选项。注册后，我们可以使用以下方式来调用组件：

```
<tagName></tagName>
```
## 4.2. 组件名大小写
定义组件名的方式有两种：

- 使用 kebab-case
```
Vue.component('my-component-name', { /* ... */ })
```
当使用 kebab-case (短横线分隔命名) 定义一个组件时，你也必须在引用这个自定义元素时使用 kebab-case，例如` <my-component-name>`


- 使用 PascalCase
```
Vue.component('MyComponentName', { /* ... */ })
```
当使用 PascalCase (首字母大写命名) 定义一个组件时，你在引用这个自定义元素时两种命名法都可以使用。也就是说 `<my-component-name> 和 <MyComponentName>` 都是可接受的。注意，尽管如此，直接在 DOM (即非字符串的模板) 中使用时只有 kebab-case 是有效的
## 4.3. 全局组件
注册一个简单的全局组件 runoob，并使用它：

```
<div id="app">
    <runoob></runoob>
</div>
 
<script>
// 注册
Vue.component('runoob', {
  template: '<h1>自定义组件!</h1>'
})
// 创建根实例
new Vue({
  el: '#app'
})
</script>
```
## 4.4. 局部组件
注册一个简单的局部组件 runoob，并使用它：

```
<div id="app">
    <runoob></runoob>
</div>
 
<script>
var Child = {
  template: '<h1>自定义组件!</h1>'
}
 
// 创建根实例
new Vue({
  el: '#app',
  components: {
    // <runoob> 将只在父模板可用
    'runoob': Child
  }
})
</script>
```
# 5. 自定义组件
如：
![](_v_images/20191120151223945_26378.png =1400x)
让他们有下面这种效果
![](_v_images/20191120151333769_12332.png =1210x)

```
<input v-focus>
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})
```



# 6. 实例生命周期钩子
每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。

比如 created 钩子可以用来在一个实例被创建之后执行代码：

```
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```
也有一些其它的钩子，在实例生命周期的不同阶段被调用，如 mounted、updated 和 destroyed。生命周期钩子的 this 上下文指向调用它的 Vue 实例。

```
不要在选项属性或回调上使用箭头函数，比如 created: () => console.log(this.a) 或 vm.$watch('a', newValue => this.myMethod())。因为箭头函数并没有 this，this 会作为变量一直向上级词法作用域查找，直至找到为止，经常导致 Uncaught TypeError: Cannot read property of undefined 或 Uncaught TypeError: this.myMethod is not a function 之类的错误。
```

# 7. 生命周期
![](_v_images/20191120112110599_23241.png)


