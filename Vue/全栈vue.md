# 开发环境

##  vscode智能提示功能
##  调试
###  默认支持node调试，F5启动调试;
### 启动网页调试
 需要安装debugger for chrome, 启动本地服务器:
全局安装http-server,运行命令yarn -g http-server ，http-server启动服务

## 扩展
安装vetur（可以高亮显示代码）和vue vscode snippets（智能快速代码填充）
## 小结
vs快捷生成html: !+enter
vs快速生成vue单文件结构：vbase
输入vfor快速生成循环列表

# Vue设计思想
* 数据驱动应用
* MVVM模式践行者
--------------------------------------------------
![](全栈vue_files/1.png)

# 模板语法基础
## 插入普通文本和属性绑定：{{}} 和 v-bind（即:）
## 列表渲染
```javascript
<div v-for="c in courses" :key="c">
{{c}}
</div>
```
## 用户输入
## 计算属性和watch
计算属性有缓存性：如果值没有发生变化，则页面不回重新渲染;
默认情况下watch初始化时不执行
## 事件名称用羊肉串方式编写，避免用驼峰式，因为HTML不区分大小写
# 组件
## 组件传值
最简单的方式是通属性（props）传值
子组件向父组件传值，通过自定义事件和事件监听
子组件向父组件派发事件，是通过vue的$emit方法派发
## 组件上使用v-model
v-model默认情况会转化成@input，还表示:value属性值的传递
```javascript
v-model="course" 
//相当于
value="course" @input="($event){course=$event}"  //$event表示要传递出来的参数
```

v-model=“dataProperty”其实就是value="dataProperty" @input="dataProperty = $event.target.value"的简写，因此自定义的组件可以很容易实现v-model的支持

## 组件复用

同样的路由重复访问路由时，组件是不会重新created的生命周期，为解决这种情况，使组件重新创建更新，可以写一个关于当前路由$router的自定义
```javascript
watch:{
	$router(){
		imediate:true,
		handler(){
			console.log('test');
		}
	}
}
```

...属性的展开运算符

vue的核心思想是一个驱动的理念

## 事件相关
### vm.$on
典型实例：事件总线
通过在Vue原型上添加一个Vue实例作为事件总线，实现组件间通信
Vue.prototype.$bus=new Vue()
## 插件
vue.js实现一个install方法，这个方法的第一个参数是Vue构造器，第二个参数是一个可选的选项参数
# 工程化开发
## vue cli
使用vue serve和vue build命令对于单个*.vue文件进行快速原型开发。
### 安装@vue/cli-service-global扩展

```javascript
npm install -g @vue/cli-service-global
```
### vue serve
启动一个服务并允许原型
```javascript
vue serve hello.vue
```
### 创建项目
vue create
### 项目目录结构
默认情况，在src下写程序，\src\components是程序的通用组件，app.vue是整个程序的入口
main.js是程序的入口文件，所有配置文件在package.json做组织。
项目中的public目录将来会作为开发服务器的静态路径，这里面素材webpack是不会处理的。
### 插件
vue cli一套基于插件的架构
### 静态地址处理
如果应用没有部署在域名根目录下，需要为URL配置publicPath前缀
```javascript
// vue.config.js
module.exports = {
publicPath: process.env.NODE_ENV === 'production'
? '/cart/'
: '/'
}
```
### Scoped css
当 <style> 标签有 scoped 属性时，它的 CSS 只作用于当前组件中的元素。
```javascript
<style scoped>
.red {
color: red;
}
</style>
```
其原理是通过使用 PostCSS 来实现以下转换：
```javascript
<template>
<div class="red" data-v-f3f3eg9>hi</div>
</template>
<style>
.red[data-v-f3f3eg9] {
color: red;
}
</style>
```
深度作用选择器：使用 >>> 操作符可以使 scoped 样式中的一个选择器能够作用得“更深”.
Sass 之类的预处理器无法正确解析 >>> 。这种情况下你可以使用 /deep/ 或 ::v-deep 操作符
取而代之.
### CSS Module
用于模块化和组合css的系统
添加module

```javascript
<style module lang="scss">
.red {
color: #f00;
}
.bold {
font-weight: bold;
}
</style>
```
模板中通过$style.xx访问
```javascript
<a :class="$style.red">awesome-vue</a>
<a :class="{[$style.red]:isRed}">awesome-vue</a>
<a :class="[$style.red, $style.bold]">awesome-vue</a>

```
### 数据访问相关
数据模拟
使用开发服务器配置before选项，可以编写接口，提供模拟数据
```javascript
devServer:{
	before(app) {
		app.get('/api/courses', (req, res) => {
			res.json([{ name: 'web全栈', price: 8999 }, { name: 'web高级', price:
			8999 }])
		})
	}
}

```
调用
```javascript
import axios from 'axios'
export function getCourses() {
	return axios.get('/api/courses').then(res => res.data)
}

```
设置开发服务器代理选项可以有效避免调用接口时出现跨域的情况
```javascript
devServer: {
	proxy: 'http://localhost:3000'
}

```
测试接口
```javascript
// 需要安装express：npm i express
const express = require('express')
const app = express()
app.get('/api/courses', (req, res) => {
	res.json([{ name: 'web全栈', price: 8999 }, { name: 'web高级', price: 8999 }])
})
app.listen(3000)

```
### 路由

#### 路由基础



#### 编程导航

除了使用route-link实现路由导航，还可以用编程的方式实现

router.push(location, onComplete?, onAbort?)
#### 路由守卫

全局守卫

路由独享守卫

组件内守卫

#### 动态路由

通过router.addRoutes(routes)方式动态添加路由
```javascript
// Login.vue用户登录成功后动态添加/about
login() {
window.isLogin = true;
this.$router.addRoutes([
{
path: "/about", //...
}
]);
const redirect = this.$route.query.redirect || "/";
this.$router.push(redirect);
}

```
#### 路由组件缓存

利用keepalive做组件缓存，保留组件状态，提高执行效率
```javascript
//缓存about组件
<keep-alive include="about">
<router-view></router-view>
</keep-alive>

```
* 使用include或exclude时要给组件设置name
* 两个特别的生命周期：activated、deactivated
### 派生状态
可以使用getters从store的state中派生出一些状态
### vuex
如果没有异步操作，直接mutation就可以，异步的话，就需要使用action
## vue ui

# Vue组件化实践

$listeners实现子组件监听，回调函数在父组件实现。它的实用场景是在一些更高层级的封装

## 组件传值

**父组件向子组件**

 prop

**$attrs/$listeners**

父组件向子组件传未声明的prop属性，用$attrs访问prop以外的未声明的属性。

```
// child：并未在props中声明foo
<p>{{$attrs.foo}}</p>
// parent
<HelloWorld foo="foo"/>
```

使用$listeners在子组件中监听，回调函数在祖辈组件中设置。

**$parent/$root:**

兄弟组件之间通信可通过共同祖辈搭桥，$parent或$root。

```
// brother1
this.$parent.$on('foo', handle)
// brother2
this.$parent.$emit('foo')
```

**$children**

父组件可以通过$children访问子组件实现父子通信。

```
// parent
this.$children[0].xx = 'xxx'
```

**子组件向父组件：**

自定义事件

```
// child
this.$emit('add', good)
// parent
<Cart @add="cartAdd($event)"></Cart>
```

**事件总线**

任意两个组件之间传值常用事件总线 或 vuex的方式.

```
// Bus：事件派发、监听和回调管理
class Bus {
    constructor(){
    this.callbacks = {}
	}
    $on(name, fn){
        this.callbacks[name] = this.callbacks[name] || []
        this.callbacks[name].push(fn)
    }
    $emit(name, args){
        if(this.callbacks[name]){
        this.callbacks[name].forEach(cb => cb(args))
        }
    }
}
// main.js
Vue.prototype.$bus = new Bus()
// child1
this.$bus.$on('foo', handle)
// child2
this.$bus.$emit('foo')

```

**refs**

获取子节点引用

```
// parent
<HelloWorld ref="hw"/>
mounted() {
	this.$refs.hw.xx = 'xxx'
}
```

**provider/inject：**

实现祖辈和后代传值。主要为高阶插件/组件提供用例，不用于应用程序。

**vuex**

创建唯一的全局数据管理者store，通过它管理数据并通知组件状态变更。

## 插槽

插槽语法是Vue 实现的内容分发 API，用于复合组件开发。该技术在通用组件库开发中有大量应用。

父组件声明的内容在子组件中使用。

没有名字的template放在匿名插槽

**匿名插槽**

```
// comp1
<div>
<slot></slot>
</div>
// parent
<comp>hello</comp>
```

**具名插槽**

将内容分发到子组件指定位置

```
// comp2
<div>
<slot></slot>
<slot name="content"></slot>
</div>
// parent
<Comp2>
<!-- 默认插槽用default做参数 -->
<template v-slot:default>具名插槽</template>
<!-- 具名插槽用插槽名做参数 -->
<template v-slot:content>内容...</template>
</Comp2>
```

**作用域插槽**

分发内容要用到子组件中的数据

```
// comp3
<div>
<slot :foo="foo"></slot>
</div>
// parent
<Comp3>
<!-- 把v-slot的值指定为作用域上下文对象 -->
<template v-slot:default="slotProps">
来自子组件数据：{{slotProps.foo}}
</template>
</Comp3>
```

## 组件构造函数获取

1. Vue.extend()

2. 通过Vue构造函数创建实例，此时的根组件的孩子即组件

   ```
    const vm = new Vue({
       // h是createElement, 返回VNode，是虚拟dom
       // 需要挂载才能变成真实dom
       render: h => h(Component, {props}),
     }).$mount() // 不指定宿主元素，则会创建真实dom，但是不会追加操作
   
    const comp = vm.$children[0]
   ```

   

# Vue全家桶原理分析

思维导图： https://www.processon.com/view/link/5e146d6be4b0da16bb15aa2a#map

插件其实就是一个对象，里面实现install方法  {install()}
纯运行时版本，不存在编译器，描述组件不能用template，只能用render函数
在vs code安装code runer 可以快捷调试js

##  实现Vue Router插件

Vue Router 是 Vue.js 官方的路由管理器.

安装： vue add router

核心步骤：

* 步骤一：使用vue-router插件，router.js

```
import Router from 'vue-router'
Vue.use(Router)

```

* 步骤二：创建Router实例，router.js

  ```
  export default new Router({...})
  ```

* 步骤三：在根组件上添加该实例，main.js

  ```
  import router from './router'
  new Vue({
  router,
  }).$mount("#app");
  ```

* 步骤四：添加路由视图，App.vue

  ```
  <router-view></router-view>
  ```

* 导航

  ```
  <router-link to="/">Home</router-link>
  <router-link to="/about">About</router-link>
  ```

**vue-router源码实现**

**需求分析**

* 作为一个插件存在：实现VueRouter类和install方法

* 实现两个全局组件：router-view用于显示匹配组件内容，router-link用于跳转 

* 监控url变化：监听hashchange或popstate事件 

* 响应最新url：创建一个响应式的属性current，当它改变时获取对应组件并显示



```
let Vue
class KvueRouter{
	constructor(options){
        this.$option=options
        //创建响应式的current属性，Vue帮助方法库Util可以给指定的对象设置一个key，实现数据响应式
        Vue.util.defineReactive(this,'current','/')
        // this.current='/'
        //创建一个路由映射表
        this.routeMap = {}
        options.routes.forEach(route => {
            this.routeMap[route.path]=route
            // if(route.path=== this.$router.current){
            //     component=route.component
            // }
        });

        //监控url变化
        window.addEventListener('hashchange',this.onHashChange.bind(this))
        window.addEventListener('load',this.onHashChange.bind(this))
    }
    onHashChange() {
         console.log(window.location.hash);
        this.current=window.location.hash.slice(1)

    }
}
KvueRouter.install=function(_vue){
    Vue=_vue //为响应式保存Vue构造函数
    Vue.mixin({
        beforeCreate(){
            //  console.log(this)
            if(this.$options.router){
                Vue.prototype.$router=this.$options.router
            }
        }
        
    })
    Vue.component('router-link',{
        props:{
            to:{
                type:String,
                required:true
            }
        },
        render(h){
            //<a href='#\'></a>
            //<router-link to="/">Home</router-link> |
            console.log(this.$slots)
            return h('a',{attrs:{'href':'#'+this.to}},this.$slots.default)
        }
    }),
    Vue.component('router-view',{
        render(h){
            let comp=null;
            // console.log( this.$router.$option)
            // console.log( this.$router.current)
            // this.$router.$option.routes.forEach(route => {
            //     if(route.path=== this.$router.current){
            //         component=route.component
            //     }
            // });
            const {routeMap,current} = this.$router
            comp = routeMap[current].component || null
            return h(comp)
            // return h('div',{},'router view')
        }
    })
```

### 嵌套路由实现

## Vuex

### vuex与react的redux区别

vuex是通过vue的响应式系统完成数据变更。redux数据响应原则是，首先同样是通过dispatch一个action尝试去改变状态，变更后，底层会有更新函数作订阅，其实是利用发布订阅模式，把更新函数通过订阅的方式传给redux, 这样redux如果发现数据发生变化就会把所有的订阅函数都执行一遍。

### 安装

```
vue add vuex
```

 ## 总结

VueRouter: 路由守卫；生命周期钩子;模式匹配

Vuex: 插件机制；模块化实现；映射函数



# 手撸Vue

![](全栈vue_files/1.png)

响应式：监听数据变化并在视图中更新

* Object.defineProperty()  (vue 2.0)
* Proxy (vue 3) 

模板引擎：提供描述视图的模板语法

* 插值：{{}}
* 指令：v-bind, v-on, v-model, v-for, v-if

渲染：如何将模板转换为html

* 模板 => vdom => dom

## 实现数组响应式

1. 找到数组原型
2. 覆盖那些能够修改数组的更新方法，使其能够通知更新
3. 将得到的新原型设置到数组实例原型上

# 源码剖析vue
## 问题
* __dirname，指当前模块的目录名
* path.resolve([…paths])里的每个参数都类似在当前目录执行一个cd操作，从左到右执行，返回的是最后的当前目录
path.resolve('/foo/bar', '/tmp/file/');相当于：
```javascript
cd /foo/bar //此时路径为 /foo/bar
cd /tmp/file/ //此时路径为 /tmp/file
```

创建vue实例时，如果有el选项，不用$mount方法也是可以在页面渲染的，但如果只有template选项就需要$mount挂载方法了。

vue实例化过程中，render优先级是最高的，render>template>el

## 初始化流程

src\platforms\web\entry-runtime-with-compiler.js

入口文件，覆盖$mount: 处理template和el

src\platforms\web\runtime\index.js

定义$mount

src\core\global-api\index.js

定义全局api

src\core\instance\index.js

定义构造函数

定义实例方法

```
initMixin(Vue)  //通过该方法添加_init方法
stateMixin(Vue)  //$set,$delete,$watch
eventsMixin(Vue) //$emit,$on,$off,$once
lifecycleMixin(Vue) //_update(),$forceupdate(),$destroy()
renderMixin(Vue)  //_render(),$nextTick()
```



src\core\instance\init.js

初始化方法_init定义的地方

```
initLifecycle(vm)
initEvents(vm)
initRender(vm)
callHook(vm, 'beforeCreate')
initInjections(vm) // resolve injections before data/props
initState(vm)
initProvide(vm) // resolve provide after data/props
callHook(vm, 'created')
```



src\core\instance\state.js

initdata, 获取data，设置代理，启动响应式

src\core\observer\index.js

vue 2.0后，一个组件只有一个watcher

vue 2.0响应式缺点：

1. 递归遍历性能会受影响，vue 3使用proxy
2. api不统一（对象和数组用两套方案）

## 初始化过程

new Vue() => this._init(options) => $mount =>mountComponent() => _render()  => _update()

调用init            初始化各种属性    调用mountComponent       声明updateComponent、创建Watcher

_render()获取虚拟dom , _update()把虚拟dom转化为真实dom



## Vue响应式系统

思维导图： https://www.processon.com/view/link/5d1eb5a0e4b0fdb331d3798c#map

将普通的js对象传入vue实例，Vue会使用Object.defineProperty将对象的属性转化成getter/setters. Object.defineProperty是ES5才有的特性，这也是为什么Vue不支持IE8以及更低的版本。

getter/setters对用户是不可见的，但在后台当属性被访问或者被修改时，它们能够使Vue执行依赖收集和

更新通知。

![Image for post](https://miro.medium.com/max/2220/1*nO3o2h5llwzP05yGbiqxig.png)

# 服务端渲染

##  目标

* vue ssr 原生实现
* nuxt.js

## 资源

https://ssr.vuejs.org/

https://nuxtjs.org/

## 项目结构中的文件说明

项目中两个入口文件：

* entry-server.js
* entry-client.js

entry-server.js用于处理客户端发送过来的请求，负责渲染首屏逻辑。

entry-client.js在浏览器中运行，它会创建一个vue实例，再执行挂载，这样程序在前端被激活，变成了SPA. 

只有首屏才会刷新，它是在服务器里返回的，首屏加载完后，前端激活代码再执行的。在页面中点击来回切换是不会向服务器再发送请求的，是一个SPA页面。

router又3种模式：hash/history（基于浏览器），abstract（用于服务端）。

创建一个express服务器，将vue ssr集成进来：

```

// 导入express作为渲染服务器
const express = require("express");
// 导入Vue用于声明待渲染实例
const Vue = require("vue");
// 导入createRenderer用于获取渲染器
const { createRenderer } = require("vue-server-renderer");
// 创建express实例
const app = express();
// 获取渲染器
const renderer = createRenderer();
// 待渲染vue实例
const vm = new Vue({
data: {
name: "开课吧"
},
template: `
<div >
<h1>{{name}}</h1>
</div>
`
});
app.get("/", async function(req, res) {
// renderToString可以将vue实例转换为html字符串
// 若未传递回调函数，则返回Promise
try {
const html = await renderer.renderToString(vm);
res.send(html);
} catch (error) {
res.status(500).send("Internal Server Error");
}
});
app.listen(3000, () => {
// eslint-disable-next-line no-console
console.log("启动成功");
});
```

## 构建应用

引入webpack进行打包，webpack做两个事情，最终渲染两个输出：一个是服务器的包（server bundle），一个是客户端的包（Client Bundle）.

vue 3的项目配置要在根目录下的vue.config.js作配置。

运行脚本传环境变量跨平台，要安装依赖 npm i cross-env -D 。

安装favicon中间件，处理/favicon请求。

### 构建流程

![image-20210223190538240](D:\File\Learning-Resource\images\vue\image-20210223190538240.png)

###  代码结构

```
src
├── main.js # 用于创建vue实例
├── entry-client.js # 客户端入口，用于静态内容“激活”
└── entry-server.js # 服务端入口，用于首屏内容渲染
```



## ssr优缺点：

* 优点：seo;首屏时间

* 缺点：开发逻辑复杂；

  ​			开发条件限制, 比如部分生命周期不能用，一些三方库不能用，只能用beforcreated,created的生命钩子；

  ​			服务器负载大，每个请求都要创建路由，store实例，需要做优化处理，如缓存处理，负载均衡。

## 已存在的SPA应用改成ssr的方案：



# Typescript

TS是ES6,ES5的超集

特性：

交叉类型:

```
type First = {first:number}
type Second = {second:number}
//扩展新的类型
type FirstAndSecond = First & Second
function fn4():FirstAndSecond{
	return {first:1,second:2}
}
```



联合类型：

```
let union: string | number;
union = '1'
union = 1
```

有两种方式在Vue组件上实现TypeScript



# Vue项目最佳实践

比较常用熟知的社区项目有Vue element admin

## 项目配置

###  vue.config.js

```
const port = 7070;

module.exports = {
publicPath: '/best-practice', // 部署应用包时的基本 URL
devServer: {
	port,
}
};
```



它采用node.js语法编写；

导出的对象使用vue cli解析，并与webpack,devserver打交道；

配置与项目本身，webpack相关的配置；

部署时，上下文路径配publicPath;

与webpack相关的配置configureWebpack;

通过链接访问特定的加载程序时，vue inspect将非常有用。

####  链式配置

除了基础的配置外，还有高级的链式配置。比如加svg-sprite-loader可以解决图标自动加载的问题。

最基础的是先导入素材：

````
<template>
    <svg>
        <use xlink:href="#icon-wx" />
    </svg>
</template>
<script>
    import '@/icons/svg/wx.svg'
    export default {}
</script>

````

注: 打包原理：打包完后的前端页面可以看到svg标签里有loader创建的元件库symbol。

工程化方式自动导入：

* 创建icons/index.js

  ```
  const req = require.context('./svg', false, /\.svg$/)
  req.keys().map(req);
  ```

require.context是webpack的api,通过执行require.context函数获取一个特定的上下文,主要用来实现自动化导入模块。

require.context函数接受三个参数

1. directory {String} -读取文件的路径
2. useSubdirectories {Boolean} -是否遍历文件的子目录
3. regExp {RegExp} -匹配文件的正则



* 创建SvgIcon组件，components/SvgIcon.vue

  ```
  <template>
  <svg :class="svgClass" v-on="$listeners">
  	<use :xlink:href="iconName" />
  </svg>
  </template>
  <script>
  export default {
      name: 'SvgIcon',
      props: {
          iconClass: {
              type: String,
              required: true
      	},
          className: {
              type: String,
              default: ''
          }
      },
      computed: {
          iconName() {
          	return `#icon-${this.iconClass}`
      	},
          svgClass() {
              if (this.className) {
                  return 'svg-icon ' + this.className
              } else {
                  return 'svg-icon'
              }
          }
      }
  }
  </script>
  <style scoped>
  .svg-icon {
      width: 1em;
      height: 1em;
      vertical-align: -0.15em;
      fill: currentColor;
      overflow: hidden;
  }
  </style>
  ```

  

小结：1. git reset --hard <tag Name>

			2. 全局路由守卫配置
   			3. axios请求，本地mock, 线上mock（easy mock）,服务器api
            			4. Vue CLI 项目有三个模式：development, test, production
                  			5. 可以通过传递 `--mode` 选项参数为命令行覆写默认的模式(比如vue-cli-service build --mode development)

 .map(ch web ild 全栈架构师 => mapComponent(child))