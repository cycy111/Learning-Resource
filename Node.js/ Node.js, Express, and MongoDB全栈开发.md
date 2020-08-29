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
 配置完前端应用后，在子目录backend将配置后端。
## 启动Node server
Node是运行时，它使我们可以用JavaScript编写所有服务器端任务，例如业务逻辑，数据持久性和安全性。它还添加了普通浏览器JavaScript不具备的功能，例如，使您可以访问本地文件系统。