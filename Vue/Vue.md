# Vuex
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