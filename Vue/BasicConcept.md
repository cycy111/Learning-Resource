- 计算属性是基于它们的响应式依赖进行缓存的.
- 计算属性默认只有 getter，不过在需要时你也可以提供一个 setter.
- 当需要在数据变化时执行异步或开销较大的操作时，自定义的侦听器更有用.
- 使用 watch 选项允许我们执行异步操作 (访问一个 API)，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。
- 除了 watch 选项之外，您还可以使用命令式的 vm.$watch API
- 组件是可复用的 Vue 实例，所以它们与 new Vue 接收相同的选项，例如 data、computed、watch、methods 以及生命周期钩子等。仅有的例外是像 el 这样根实例特有的选项。
- Lodash是一个一致性、模块化、高性能的 JavaScript 实用工具库。
- 一个组件上的 v-model 默认会利用名为 value 的 prop 和名为 input 的事件
- yarn --  the package manager 
-  Vue CLI projects make use of Webpack: a module bundler.
- To preprocess files, Webpack uses loaders that transform your source code. For example, Vue has their own loader because of the .vue files.

### Vue CLI 有几个独立的部分:
1. CLI (@vue/cli) 是一个全局安装的 npm 包,提供了终端里的 vue 命令.
2. CLI 服务 (@vue/cli-service) 是一个开发环境依赖,它是一个 npm 包，局部安装在每个 @vue/cli 创建的项目中。

    *CLI 服务是构建于 webpack 和 webpack-dev-server 之上的。它包含了:*

    - 加载其它 CLI 插件的核心服务；
    - 一个针对绝大部分应用优化过的内部的 webpack 配置；
    - 项目内部的 vue-cli-service 命令，提供 serve、build 和 inspect 命令。

3. CLI 插件是向你的 Vue 项目提供可选功能的 npm 包
