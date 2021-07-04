docker 极大简化了部署过程，一次封装，到处执行。基于Linux的高效、敏捷、轻量级的容器方案。基于操作系统的虚拟化技术。

https://github.com/su37josephxia/docker_ci

git clone ..../docker_ci.git

docker-compose up

## Dockerfile格式

```
# Comment
INSTRUCTION arguments
```



## 简单Nginx服务

```
# 拉取官方镜像 - 面向docker的只读模板
docker pull nginx

# 查看
docker images nginx

# 启动镜像
mkdir www
echo 'hello docker!!' >> www/index.html

# 启动
# www目录里面放一个index.html
docker run -p 8000:80 -v $PWD/www:/usr/share/nginx/html nginx
# 后台启动
docker run -p 80:80 -v $PWD/www:/usr/share/nginx/html -d nginx
```



查看docker进程

```
docker ps
docker stop [containerid] //停止进程
docker start [containerid] //启动进程
docker ps -a 查看已存在的使用进程，包括不在运行的进程
```



进入docker内部，即访问它的伪终端。docker内部实际上是一个虚拟机，也是一个小的Linux服务器。

```
//进入docker内部
docker exec -it [containnerId] /bin/bash
```

## Docker的运行过程

* 镜像（Image） 

  面向Docker的只读模板 

* 容器（Container） 

  镜像的运行实例 

* 仓库 (Registry) 	

  存储镜像的服务器

![image-20210224161143823](D:\File\Learning-Resource\images\docker.png)

## Dockerfile定制镜像

* 定制自己的web服务器

  ```
  #Dockerfile
  FROM nginx:latest
  RUN echo '<h1>Hello, Kaikeba!</h1>' > /usr/share/nginx/html/index.html
  
  ```

  ```
  # 定制镜像
  docker build -t nginx:kaikeba .  //:冒号后面指定镜像版本，.表示当前目录下的dockerfile文件
  # 运行
  docker run -p 80:80 nginx:kaikeba
  ```

  

## 定制NodeJS镜像

定制一个程序NodeJS镜像

```
npm init -y
npm i koa -s
```

```
// package.json
{
    "name": "myappp",
    "version": "1.0.0",
    "main": "app.js",
    "scripts": {
    	"test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "description": "myappp",
    "dependencies": {
    	"koa": "^2.7.0"
    }
}
```

```
// app.js
const Koa = require('koa')
const app = new Koa()
app.use(ctx => {
	ctx.body = 'Hello NodeJS'
})
app.listen(3000, () => {
	console.log('app started at http://localhost:3000/')
})
```

```
#Dockerfile
#制定node镜像的版本
FROM node:10-alpine
#移动当前目录下面的文件到app目录下
ADD . /app/
#进入到app目录下面，类似cd
WORKDIR /app
#安装依赖
RUN npm install
#对外暴露的端口
EXPOSE 3000
#程序启动脚本
CMD ["node", "app.js"]
```

```
# 定制镜像
docker build -t mynode .
# 运行
docker run -p 3000:3000 -d mynode
```

## PM2镜像定制

一般使用PM2运行node

![image-20210514105431155](D:\File\Learning-Resource\images\image-20210514105431155.png)

PM2运行的时候，一般会使用process.yml文件，用于描述它的运行

```
//拷贝node到pm2
# cp -R node pm2
```

Pm2 - 利用多核资源

```
# .dockerignore
node_modules
```

```
// process.yml
apps:
    - script : app.js
    instances: 2
    watch : true
    env :
    NODE_ENV: production
```

```
# Dockerfile
FROM keymetrics/pm2:latest-alpine
WORKDIR /usr/src/app
ADD . /usr/src/app
RUN npm config set registry https://registry.npm.taobao.org/ && \
npm i
EXPOSE 3000
#pm2在docker中使用命令为pm2-docker
CMD ["pm2-runtime", "start", "process.yml"]
```

```
# 定制镜像
docker build -t mypm2 .
# 运行
docker run -p 3000:3000 -d mypm2
```

## docker-compose

Compose项目是 Docker 官方的开源项目，负责实现对 Docker 容器集群的快速编排。

```
#docker-compose.yml
version: '3.1'
services:
    mongo:
        image: mongo
        restart: always
        ports:
        - 27017:27017
    mongo-express:
        image: mongo-express
        restart: always
        ports:
        - 8000:8081
```

```
#mkdir mongo
#cd mongo
#vi docker-compose.yml
#docker-compose up
```

## 实战nginx

将一个前后端分离的项目同步到阿里云服务器时，vs code上的deploy插件支持同步功能



正向代理：

![代理](D:\File\Learning-Resource\images\proxy.jpg)

反向代理：

![reverse代理](D:\File\Learning-Resource\images\reverseProxy.jpg)

反向代理服务器会帮我们把请求转发到真实的服务器那里去。Nginx就是性能非常好的反向代理服务器，用来做负载均衡。**正向代理**代理的对象是客户端，**反向代理**代理的对象是服务端。