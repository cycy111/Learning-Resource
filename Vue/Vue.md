# Vue Cli创建项目
## 背景
构建工具（Gulp, Webpack等）和命令行的出现，改进了以前在前端开发中很多东西都需要手动配置，比如根据文件内容来包含CSS，为了缩小加载时间最小化JavaScript文件的大小。
由于这些构建工具存在的复杂性，企业级应用程序必须使用命令行界面（CLI）.
## 安装Vue Cli
```javascript
npm install -g @vue/cli
// OR
yarn global add @vue/cli
```
检查是否安装成功：
```
vue --version
```
## 创建项目
开始创建项目：
<img src="../images/vue/createvueprj.png">
构建完成的项目结构目录：
<img src="../images/vue/filestruc.jpg">
* node_modules- 包含能在应用中实现很多东西的所有依赖，它不需要提交到代码库，是由npm来管理的。
* public- 此文件夹包含favicon.ico和index.html,此处包含的所有内容将直接通过根网址提供。
* index.html将用于生成应用程序的其余部分。
* src - 大部分代码将在这里进行
* gitignore - 包含不被提交到仓库的文件或目录列表，像常见的/dist目录（每一次项目构建都会自动生成），node_modules目录（每一次npm install  or  yarn install都会自动创建）。
* package.json- 项目的基本配置。包含像项目名称，版本的元数据。包含重要信息，例如可以运行（即serve和build）脚本以及项目需要哪些依赖项。
	+ Serve- 该脚本用于启动本地开发环境
	+ Build- 该脚本负责创建最终代码工件的脚本，该工件将交付给客户或用户。

src目录：
* Assets - 您可以在此目录中放置诸如图像和可能需要参考的其他必需资产之类的内容
* Components - 放项目用到的组件
* main.js - 这是设置高级Vue配置选项的地方。
启动项目：
```
npm run serve
or
yarn run serve
```
# Vue Router管理导航
使用 Vue CLI工具给项目加vue router
```
vue add router
```
添加完成后会新产生一个文件router/index.js. main.js也会有些变化。
Vue Router使用两个组件：
* <router-view></router-view> - 定义我们在每条路由中定义的组件将出现在页面上的区域。

* <router-link></router-link> -
# Vuex
## 用Vuex创建集中数据存储
Vuex是Vue.js应用程序的状态管理模式和库。 换句话说，Vuex的唯一目的是帮助创建集中化的数据存储，该存储将作为应用程序的SSOT( single source of truth )。
使用Vue CLI tool 安装Vuex
```javascript
vue add vuex
```
安装完成后， 会在应用对src/main.js文件产生变化：
```javascript
import Vue from 'vue'
import App from './App.vue'
import store from './store'

Vue.config.productionTip = false

new Vue({
    store,
    render: h => h(App)
}).$mount('#app')
```
这里插件引入了store,它作为一个新的配置在Vue实例中。

与在Vue实例中配置data属性类似，Vuex在src/store.js这样配置store：
```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
    state: {

    },
    mutations: {

    },
    actions: {

    }
})
```
**Vue.use(Vuex)**是Vue插件系统中的语法，它允许向Vue实例添加全局功能。
Vue 实例通过new Vue()设置，Vuex 通过new Vuex.Store()设置。新的Vuex store实例接收一个单一的配置对象。
## 存取数据
使用$store属性访问
```javascript
<template>
    <p>The date stored in Vuex is {{ $store.state.year }}-{{ $store.state.month }}-{{ $store.state.day }}.</p>
</template>
```
## 使用mapState
通过$store在组件中访问数据看起来有点乱，Vuex提供了一种更简单的方式mapState：匹配state到组件中的变量。
这种方式通过将我们需要请求的state属性加到组件的计算属性。
```javascript
export default new Vuex.Store({
    state: {
        month: 08,
        day: 12,
        year: 2008
    }
}
```
```javascript
<template>
    <p>The date stored in Vuex is {{ year }}-{{ month }}-{{ day }}.</p>
</template>

<script>
import { mapState } from 'vuex'

export default {
    computed: {
        ...mapState(['year', 'month', 'day'])
    }
}
</script>
```
...扩展符是通常的做法，是允许定义其他计算的属性。
为避免名称冲突,可以使用将数组参数换成对象参数给 Vuex state变量指定一个不一样的名称：
```javascript
<template>
    <p>The date stored in Vuex is {{ customYear }}-{{ uniqueMonth }}-{{ day }}.</p>
</template>

<script>
import { mapState } from 'vuex'

export default {
    computed: {
        ...mapState({
            customYear: 'year',
            uniqueMonth: 'month',
            day: 'day'
        })
    }
}
</script>
```
## Getter
store.js初始化缺少的一个重要属性：Getter， Vuex的getters属性类似于vue对象中的计算属性。

<img src="../images/getter.png"/>

我们来定义一个格式化的日期：
```javascript
export default new Vuex.Store({
    state: {
        month: 08,
        day: 12,
        year: 2008
    },
    getters: {
        formattedDate: state => {
            return `${state.year}-${state.month}-${state.day}`
        }
    }
}
```
每一个getter都是接收state作为参数的函数，再返回一个值为后面访问。
我们可以在组件中使用mapGetters来进一步简化代码：
```javascript
<template>
    <p>The date stored in Vuex is {{ formattedDate }}.</p>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
    computed: {
        ...mapGetters(['formattedDate'])
    }
}
</script>
```
和mapState一样，你也可以使用一个对象来为你正在引用的getter自定义名称。
## 修改Vuex中的数据
### Mutation
了解完在Vuex中用state和getter定义数据，用方法像mapState  和  mapGetters取数据，下一步就是管理数据，
用mutations更新修改Vuex 中的数据。
store.js文件初始化时：
```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
    state: {

    },
    mutations: {

    },
    actions: {
    
    }
})
```
mutations包含所有属性负责更改state状态的对象.
####  定义Mutation
```javascript
export default new Vuex.Store({
    state: {
        count: 0
    },
    mutations: {
        INCREASE_COUNT(state, payload) {
            state.count += Number(payload)
        }
    },
    actions: {

    }
})
```
####  提交Mutation
调用mutation 不像普通函数，它使用commit。当要提交mutation，这个动作取两个参数：
* mutation 的名称
* Payload（可选）

```javascript
this.$store.commit('INCREMENT_COUNT', 2)
```
当一个mutation提交后，修改便会立刻产生，换句话说就是Vuex mutations 是同步的，这样就不能从API中取数据。
### Actions
Actions是我们用来协调突变背后逻辑的主要工具。Actions 类似于Vue实例中的methods属性：
<img src="../images/actions.png"/>

#### 定义Action
```javascript
export default new Vuex.Store({
    state: {
        count: 0
    },
    mutations: {
        INCREASE_COUNT(state, amount = 1) {
            state.count += Number(amount)
        }
    },
    actions: {
        incrementCount(context, amount) {
            context.commit('INCREMENT_COUNT', amount)
        }
    }
})
```
action 由它的名称，上下文参数和一个可选的payload组成。
#### Actions的作用
当我们做减法
```javascript
export default new Vuex.Store({
    state: {
        count: 0
    },
    mutations: {
        INCREASE_COUNT(state, amount = 1) {
            state.count += Number(amount)
        },
        DECREASE_COUNT(state, amount = 1) {
            state.count -= Number(amount)
        }
    },
    actions: {
        incrementCount(context, amount) {
            context.commit('INCREMENT_COUNT', amount)
        }
    }
})
```
我们需要处理一些逻辑来决定触发每个mutation