# 要领

## 安装

引入vue.js到项目中，有四种方式：

1. 作为CDN包引入
2. 下载JavaScript文件到本地
3. 使用npm安装
4. 使用官方的CLI构建项目，CLI的方式为现代前端工作流提供了包含健全功能能用到的所有可能的构建设置（比如hot-reload, lint-on-save等）

### CDN

```html
<script src="https://unpkg.com/vue@next"></script>
```

### npm

使用Vue构建大型应用程序时，建议使用npm安装方法。 它与诸如Webpack或Rollup之类的模块捆绑器很好地配对。

```sh
# latest stable
$ npm install vue@next
```

### CLI

Vue提供了一个官方的CLI，用于快速构建Single Page Applications。它为现代的前端工作流提供了包括电池在内的构建设置。 只需几分钟就可以启动带hot-reload, lint-on-save以及可用于生产的构建。

对于Vue 3，您应该使用npm上的Vue CLI v4.5，其名称为@ vue / cli。为了升级，需要全局重装@vue/cli的最新版本：

```sh
yarn global add @vue/cli
# OR
npm install -g @vue/cli
```

然后在项目里面运行：

```sh
vue upgrade --next
```

### Vite

Vite是一个Web开发构建工具，由于其本机ES模块导入方法，因此可以闪电般快速地提供代码。

使用Vite快速构建vue项目：

```sh
//使用npm
$ npm init @vitejs/app <project-name>
$ cd <project-name>
$ npm install
$ npm run dev

//使用Yarn
$ yarn create @vitejs/app <project-name>
$ cd <project-name>
$ yarn
$ yarn dev
```

## 应用程序和组件实例

### 组件实例属性

分为用户自定义属性和内置属性。用户自定义属性在组件选项methods`, `props`, `computed`, `inject` 和`setup中加入。

内置属性区别于用户自定义属性，它带有前缀$，像$attrs, $emit

## 模板语法

### 指令

指令是以v-开头的特殊属性，属性值是一个单JavaScript表达式（v-for和v-on例外）。

**参数**

某些指令可以带有一个“参数”，在指令名称后用冒号表示.  比如，v-bind指令用于以响应式方式更新HTML属性：：

```html
<a v-bind:href="url"> ... </a>  //这里的参数是href
```

另一个示例是v-on指令，它侦听DOM事件：

```html
<a v-on:click="doSomething"> ... </a>  //这里的参数是要监听的事件名称
```

**动态参数**

```html
<a v-bind:[attributeName]="url"> ... </a>  //这里attributeName是一个表达式，
```

## 数据属性和方法

一个组件的data选项是一个方法，该方法返回一个对象。作为创建一个新的组件实例的一部分，Vue调用这个方法。返回的方法会包裹在它的响应式系统里，并作为$data存储在组件实例中。为了方便起见，该对象的所有顶级属性也直接通过组件实例公开：

```js
const app = Vue.createApp({
  data() {
    return { count: 4 }
  }
})

const vm = app.mount('#app')

console.log(vm.$data.count) // => 4
console.log(vm.count)       // => 4

// Assigning a value to vm.count will also update $data.count
vm.count = 5
console.log(vm.$data.count) // => 5

// ... and vice-versa
vm.$data.count = 6
console.log(vm.count) // => 6
```

通过组件实例公开自己的内置API时，Vue使用$前缀。 它还为内部属性保留前缀_。因此要避免在data属性中使用以‘$’和‘-’开头的属性名称。

**方法**

使用methods选项添加方法到组件实例。

### 计算属性和watcher

普通method和计算属性的区别是，计算属性是基于响应式依赖。比如下面的计算属性是不会更新的,因为Date.now()不是响应式依赖项：

```js
computed: {
  now() {
    return Date.now()
  }
}
```



默认情况下，计算属性仅是getter的，但是您也可以在需要时提供setter：

```js
// ...
computed: {
  fullName: {
    // getter
    get() {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set(newValue) {
      const names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

运行vm.fullName = 'John Doe'，可以修改vm.firstname，vm.lastname.

Vue提供了一种更通用的方式来通过watch选项对数据更改做出反应。

当您要执行异步或昂贵的操作以响应更改的数据时，此watcher功能非常有用。