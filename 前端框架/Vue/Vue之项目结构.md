# 1. 项目结构
![](_v_images/20191119233341192_25656.png =2172x)

- package.json保存一些依赖信息
- config保存一些项目初始化配置
- build里面保存一些webpack的初始化配置
- index.html是我们的首页
- 最关键的代码都在src目录中，index在很多服务器语言中都是预设为首页，像index.htm，index.php等；打开build目录中的webpack.base.conf.js，会看到这样的代码

```
├── build                      // 构建相关  
├── config                     // 配置相关
├── src                        // 源代码
│   ├── api                    // 所有请求
│   ├── assets                 // 主题 字体等静态资源
│   ├── components             // 全局公用组件
│   ├── directive              // 全局指令
│   ├── filtres                // 全局 filter
│   ├── icons                  // 项目所有 svg icons
│   ├── lang                   // 国际化 language
│   ├── mock                   // 项目mock 模拟数据
│   ├── router                 // 路由
│   ├── store                  // 全局 store管理
│   ├── styles                 // 全局样式
│   ├── utils                  // 全局公用方法
│   ├── vendor                 // 公用vendor
│   ├── views                   // view
│   ├── App.vue                // 入口页面
│   ├── main.js                // 入口 加载组件 初始化等
│   └── permission.js          // 权限管理
├── static                     // 第三方不打包资源
│   └── Tinymce                // 富文本
├── .babelrc                   // babel-loader 配置
├── eslintrc.js                // eslint 配置项
├── .gitignore                 // git 忽略项
├── favicon.ico                // favicon图标
├── index.html                 // html模板
└── package.json               // package.json
```
# 2. 跟着代码走

Vue的核心架构，按照官方解释和个人理解，主要在于组件和路由两大模块，只要理解了这两大模块的思想内容，剩下API使用就只是分分钟的事情了

```
import Vue from 'vue'
import App from './App'
import router from './router'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  template: '<App/>',
  components: { App }
})

```

- 先是第一句
```
import Vue from 'vue'
```

复制代码这句很好理解，就像你要引入jQuery一样，vue就是jquery-min.js，然后Vue就是$；然后又引入了./App文件，也就是目录中和main.js同级的App.vue文件；在Vue中引入文件可以直接用import，文件后缀名可以是.vue，这是Vue自己的文件类型，之前说的webpack会将js和css文件打包，同样的道理，在webpack中配置vue插件后（项目默认配置），webpack就可以将.vue类型的文件整合打包，这和nodeJs中require差不多的道理。


说回App.vue这个文件，这是一个视图（或者说组件和页面），想象一下我们的index.html中什么也没有，只有一个视图，这个视图相当于一个容器，然后我们往这个容器中放各种各样的积木（其他组件或者其他页面）


```
import router from './router'
```

这句代码引入一段路由配置


- 接下来的 new Vue实例化，其实就相当于平时我们写js时候常用的init啦，然后声明el：'#app'，意思是将所有视图放在id值为app这个dom元素中，components表明引入的文件，即上述的App.vue文件，这个文件的内容将以<App/>这样的标签写进去#app中，总的来说，这段代码意思就是将App.vue放到#app中，然后以<App/>来指代我们的#app
- 
```
import Vue from 'vue'
import App from './App'/*引入App这个组件*/
import router from './router'/*引入路由配置*/

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',/*最后效果将会替换页面中id为app的div元素*/
  router,/*使用路由*/
  template: '<App/>',/*告知页面这个组件用这样的标签来包裹着,并且使用它*/
  components: { App }/*告知当前页面想使用App这个组件*/
})
```


# 3. 单页面组件
好了，现在打开我们的App.vue文件，在Vue中，官网叫它做组件，单页面的意思是结构，样式，逻辑代码都写在同一个文件中，当我们引入这个文件后，就相当于引入对应的结构、样式和JS代码，这不就是我们做前端组件化最想看到的吗，从前的asp、php也有这样的文件思想。
```
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'app'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```


复制代码node端之所以能识别.vue文件，是因为前面说的webpack在编译时将.vue文件中的html，js，css都抽出来合成新的单独的文件。

单页面组件会在后面的实战中完整体现，这里先不做过多描述；


看到我们文件内分为三大部分，分别是`<template><script><style>`，很好理解结构，脚本，样式；script就像node一样暴露一些接口，可以看到我们的template标签中除了一张图片之外就只有一行代码：
```
<router-view></router-view>
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view></router-view>
  </div>
</template>
```
复制代码回看我们的浏览器页面中，图片是有了，可下面的文本和链接的代码写在哪里了呢？这里就要开始涉及路由了
![](_v_images/20191119233847839_22498.png =1618x)

# 4. 路由


这里补充下路由的大致概念：传统的php路由是由服务器端根据一定的url规则匹配来返回给前端不同的页面代码，如以下地址

```
isux.tencent.com/about 和 isux.tencent.com/recruit
```

注意这里只有about和recruit，这些不带xxx.html的地址其实是服务器端经过一层封装指定到某些文件上去。同样的道理，前端也可以根据带锚点的方式实现简单路由（不需要刷新页面）


```
zhitu.isux.us/index.php/p…
```
其中#mac就是我们的锚点路由，注意开始我们在浏览器中打开的地址：
```
http://localhost:8080/#/，
```

路由让我们可以访问诸如http://localhost:8080/#/about/ 或者 http://localhost:8080/#/recruit这些页面的时候不带刷新，直接展示。现在回到我们刚才打开的App.vue文件中看这行代码
```
<router-view></router-view>
```
这句代码在页面中放入一个路由视图容器，当我们访问http://localhost:8080/#/about/的时候会将about的内容放进去，访问http://localhost:8080/#/recruit的时候会将recruit的内容放进去
![](_v_images/20191119234019056_28785.png =2168x)


如此看来，无论我们打开http://localhost:8080/#/about/ 还是http://localhost:8080/#/recruit页面中的图片都是公用部分，变得只是路由器里面的内容，那么路由器的内容谁来控制呢？
前面说的src/main.js中有一句引入路由器的代码。
```
import router from './router'
```
现在就让我们打开router目录下的js文件。
```
import Vue from 'vue'
import Router from 'vue-router'
import Hello from '@/components/Hello'
import About from '@/components/about'
import Recruit from '@/components/recruit'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'Hello',
      component: Hello
},
    {
      path: '/about',
      name: 'about',
      component: About
},
    {
      path: '/recruit',
      name: 'recruit',
      component: Recruit
}
  ]
})
```

前面先引入了路由插件vue-router，然后显式声明要用路由 Vue.use(Router) 。注意到Hello，About等都是页面（也可以是组件），接着注册路由器，然后开始配置路由


路由的配置应该一目了然，给不同的path分配不同的页面（或组件，页面和组件其实是一样的概念），name参数不重要只是用来做识别用的。看到这里就可以明白，前面说的红色框的内容，其实就是Hello里面的内容，打开components目录下的Hello.vue就能明白了。
![](_v_images/20191119234158288_8315.png =2134x)

```
      path: '/blog',
      name: 'blog',
      component: Blog,
      children: [
        {
          path: '/',
          component: page1
        },
        {
          path: 'info',
          component: page2
        }
      ]
    }
```
复制代码访问/blog的时候会访问Blog页面，Blog里面放个路由器就可以了，然后访问http://localhost:8080/#/blog/的时候会往路由容器中放置page1的内容，访问http://localhost:8080/#/blog/info的时候会往路由容器中放置page2的内容
```
//blog.vue
<template>
    <div>公用部分</div>
    <router-view></router-view>
</template>

```

# 5. vue的页面架构
![](_v_images/20191120100701284_24794.png =1386x)


以上文章来自：https://juejin.im/post/5ba9d5cce51d450e805b59b0