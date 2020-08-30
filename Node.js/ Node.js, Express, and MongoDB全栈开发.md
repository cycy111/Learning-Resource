# 课程目标
* 建基础的Express web server
* 在Express app中使用Mongoose访问MongoDB数据库
* 使用Node, Express, and MongoDB创建RESTful web API 
* 在Express app中处理用户文件
* 在Express应用中使用基于令牌的身份验证来保护所选路由
# 配置代码环境
安装Node runtime
## 安装Node
 安装最新的node版本，这会安装Node JavaScript runtime，这样就能运行Node servers。同时也会安装npm,该工具对于安装构建项目所需的软件包非常有用。
## 安装Angular 
 使用Angular CLI您可以运行开发服务器，前端代码将在其上运行。
```javascript
npm install -g @angular/cli
或者
yarn global add @angular/cli
```
使用ng version命令查看是否安装成功
## 克隆前端应用
 为课程建一个工作目录go-fullstack，再克隆前端应用到子目录frontend下。
 ```
 git clone https://github.com/OpenClassrooms-Student-Center/5614116-front-end-app.git frontend
```
 克隆完成后，运行：
 ```
 cd frontend
 npm install
 ng serve
 ```
 这将安装前端应用程序所需的所有依赖项，并将启动开发服务器。
 配置完前端应用后，在子s将包含目录backend将配置后端。
# 启动Node server
Node是运行时，它使我们可以用JavaScript编写所有服务器端任务，例如业务逻辑，数据持久性和安全性。它还添加了普通浏览器JavaScript不具备的功能，例如，使您可以访问本地文件系统。
## 初始化项目
初始化backend下的项目，在命令行运行npm init，server.js为入口。server.js将包含Node Server.在其中添加如下代码：
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    res.end('This is my server response!');
});

server.listen(process.env.PORT || 3000);
```
在命令运行node server启动服务器。
为Node开发更简单，也可以安装nodemon
```
npm install -g nodemon
```
使用命令行nodemon server
# Express
## 安装
在可能的情况下，在纯Node中编写Web服务器既费时又费力，因为我们必须手动解析每个传入的请求.使用Express框架就会使这些任务变得简单很多。
在backend下添加EXpress：
```
npm install --save express
```
创建app.js文件，在其中包含Express app:
```
const express = require('express');

const app = express();

app.use((req, res) => {
   res.json({ message: 'Your request was successful!' }); 
});

module.exports = app;
```
## 在node服务器上运行Express
修改server.js：
```javascript
const http = require('http');
const app = require('./app');

app.set('port', process.env.PORT || 3000);
const server = http.createServer(app);

server.listen(process.env.PORT || 3000);
```
## 添加中间件middleware
给Express app添加中间件
```javascript
const express = require('express');

const app = express();

app.use((req, res, next) => {
  console.log('Request received!');
  next();
});

app.use((req, res, next) => {
  res.status(201);
  next();
});

app.use((req, res, next) => {
  res.json({ message: 'Your request was successful!' });
  next();
});

app.use((req, res, next) => {
  console.log('Response sent successfully!');
});

module.exports = app;
```
这个Express app有四个middleware，显示了Express app中middleware怎么工作。
改进server.js：
```javascript
const http = require('http');
const app = require('./app');

const normalizePort = val => {
  const port = parseInt(val, 10);

  if (isNaN(port)) {
    return val;
  }
  if (port >= 0) {
    return port;
  }
  return false;
};
const port = normalizePort(process.env.PORT || '3000');
app.set('port', port);

const errorHandler = error => {
  if (error.syscall !== 'listen') {
    throw error;
  }
  const address = server.address();
  const bind = typeof address === 'string' ? 'pipe ' + address : 'port: ' + port;
  switch (error.code) {
    case 'EACCES':
      console.error(bind + ' requires elevated privileges.');
      process.exit(1);
      break;
    case 'EADDRINUSE':
      console.error(bind + ' is already in use.');
      process.exit(1);
      break;
    default:
      throw error;
  }
};

const server = http.createServer(app);

server.on('error', errorHandler);
server.on('listening', () => {
  const address = server.address();
  const bind = typeof address === 'string' ? 'pipe ' + address : 'port ' + port;
  console.log('Listening on ' + bind);
});

server.listen(port);

```
# 创建get路由
目前前端应用还没有数据，只显示 "No stuff for sale"，用下面的中间件在app.js中添加stuff：
```javascript
app.use('/api/stuff', (req, res, next) => {
  const stuff = [
    {
      _id: 'oeihfzeoi',
      title: 'My first thing',
      description: 'All of the info about my first thing',
      imageUrl: '',
      price: 4900,
      userId: 'qsomihvqios',
    },
    {
      _id: 'oeihfzeomoihi',
      title: 'My second thing',
      description: 'All of the info about my second thing',
      imageUrl: '',
      price: 2900,
      userId: 'qsomihvqios',
    },
  ];
  res.status(200).json(stuff);
});
```
与之前的文件相对，多了一个参数给use方法，一个字符串，它对应于我们要为其注册此中间件的端点（http://localhost:3000/api/stuff）。
在API路由前加如下中间件：
```javascript
app.use((req, res, next) => {
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content, Accept, Content-Type, Authorization');
  res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE, PATCH, OPTIONS');
  next();
});
```
CORS 代表 Cross Origin Resource Sharing.该标准允许我们放宽默认的安全规则，以防止在不同服务器之间进行HTTP调用。
在此项目中有两个origin ：localhost:3000 and localhost:4200。为了使两个不同源交互,我们需要向响应对象添加一些headers。
这将允许来自所有来源的所有请求访问您的API。
# 创建Post路由
从请求中提取JSON对象-我们将需要body-parser包，安装生产依赖项：
```
npm install --save body-parser
```
将其引入app.js:
```javascript
const bodyParser = require('body-parser');
```
在配置响应头后面，为项目设置json函数为全局中间件：
```javascript
app.post('/api/stuff', (req, res, next) => {
  console.log(req.body);
  res.status(201).json({
    message: 'Thing created successfully!'
  });
});
```