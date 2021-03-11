The easiest way to preview your production build locally is using a Node.js static file server

## 模式和环境变量



在Vue CLI 项目中，mode(模式)是很重要的概念。有三种模式：

- `development` is used by `vue-cli-service serve`
- `test` is used by `vue-cli-service test:unit`
- `production` is used by `vue-cli-service build` and `vue-cli-service test:e2e`

运行vue-cli-service时，环境变量会从对应文件中加载，文件中的NODE_ENV变量的值有三种情况，在生产模式下，会设置成"production"；在测试模式下，会设置成"test"；其他情况下设置成"development"。

文件.env.prod / .env.preview/... :

```
NODE_ENV = 'development'
VUE_APP_MODE = 'development'
VUE_APP_BASE_URL = 'http://apizh.668tms.com'
VUE_APP_LOGIN_URL = 'http://apizh.668tms.com:8003'
```

NODE_ENV决定应用程序的主要模式，从而决定哪种webpack config 的创建。

vue.config.js中的configureWebpack选项， configureWebpack中提供的对象最终会通过使用[webpack-merge](https://github.com/survivejs/webpack-merge)并入到webpack config。

