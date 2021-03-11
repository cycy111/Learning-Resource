docker 极大简化了部署过程，一次封装，到处执行。基于Linux的高效、敏捷、轻量级的容器方案。基于操作系统的虚拟化技术。

https://github.com/su37josephxia/docker_ci

git clone ..../docker_ci.git

docker-compose up



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