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

# 语法基础
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
:value="course" @input="($event){course=$event}"  //$event表示要传递出来的参数
```

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
### 编程导航
router.push(location, onComplete?, onAbort?)
### 路由守卫
#### 全局守卫
#### 路由独享守卫
#### 组件内守卫
### 动态路由
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
### 路由组件缓存
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
## Vue全家桶原理分析
插件其实就是一个对象，里面实现install方法  {install()}
纯运行时版本，不存在编译器，描述组件不能用template，只能用render函数
在vs code安装code runer 可以快捷调试js

# 手撸Vue

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