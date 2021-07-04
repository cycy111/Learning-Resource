nginx 有一个主进程和多个工作进程，主进程主要负责读取和计算配置文件，以及维护工作进程。工作进程主要作实际的请求处理。nginx使用基于事件的模型和依赖于操作系统的机制来有效地在工作进程之间分配请求。

工作进程数在配置文件中定义，可以针对给定的配置固定，也可以自动调整为可用的CPU内核数。

nginx及其模块的工作方式在配置文件中确定。 默认情况下，配置文件名为nginx.conf，并放置在目录/ usr / local / nginx / conf，/ etc / nginx或/ usr / local / etc / nginx中。

## 启动，停止，重载配置

一旦启动nginx，就可以通过使用-s参数调用可执行文件来对其进行控制。语法如下：

```
nginx -s signal
```

signal可以是下面其中的一个：

- `stop` — fast shutdown
- `quit` — graceful shutdown
- `reload` — reloading the configuration file
- `reopen` — reopening the log files

停止Nginx进程：

```
nginx -s quit
```

注意：该命令应该在启动Nginx的同一个账户下执行。

在配置文件中作的修改只有在使用命令重新加载配置或者重启后才能被应用。重新加载配置使用：

```
nginx -s reload
```

信号也可以借助Unix工具（例如kill实用程序）发送到nginx进程。在这种情况下，将信号直接发送给具有给定进程ID的进程。 默认情况下，nginx主进程的进程ID写入目录/ usr / local / nginx / logs或/ var / run中的nginx.pid。例如，如果主进程ID为1628，则要发送QUIT信号以导致nginx正常关闭，请执行以下命令：

```
kill -s QUIT 1628
```

获取Nginx正在运行的进程清单：

```
ps -ax | grep nginx
```

## 配置文件结构

nginx由受配置文件中指定的指令控制的模块组成。 指令分为简单伪指令和块伪指令。 一个简单的指令由名称和参数组成，这些名称和参数之间用空格分隔，并以分号（;）结尾。 块指令的结构与简单指令的结构相同，但是它以分号结尾，而不是以分号结尾，并带有一组用括号括起来的附加指令

/conf/docker.conf

```
server {
    listen 80;
    location / {
        root /var/www/html;
        index index.html index.htm;
    }
    location ~ \.(gif|jpg|png)$ {
        root /static;
        index index.html index.htm;
    }
}
```

如果块指令可以在括号内包含其他指令，则将其称为上下文（示例：event，http，server和location）。

放置在任何上下文外部的配置文件中的指令都被视为在主上下文中。 event和http指令位于主上下文中，server位于http中，location位于server中。

#后面是注释。

## 提供静态内容

Web服务器的一项重要任务是分发文件（例如图像或静态HTML页面）。实现一个示例，其中根据请求，文件将从不同的本地目录提供：/ data / www（可能包含HTML文件）和/ data / images（包含图像）。

```
server {
    location / {
        root /data/www;
    }

    location /images/ {
        root /data;
    }
}
```

应用配置，如果Nginx没有重启，可以使用命令：

```
nginx -s reload
```

如果某些操作无法按预期进行，则可以尝试在/ usr / local / nginx / logs或/ var / log / nginx目录中的access.log和error.log文件中查找原因。

## 设置代理服务器

我们将配置一个基本的代理服务器，该服务器为图像请求和本地目录中的文件提供服务，并将所有其他请求发送到代理服务器。 在此示例中，两个服务器都将在单个nginx实例上定义。

首先，添加一个新的server块来定义代理服务器

```
server {
    listen 8080;
    root /data/up1;

    location / {
    	proxy_pass http://localhost:8080/;
    }
    location ~ \.(gif|jpg|png)$ {
        root /data/images;
    }
}
```

第二个location块，该参数是一个正则表达式，它匹配所有以.gif，.jpg或.png结尾的URI。 正则表达式应以〜开头。 相应的请求将映射到/ data / images目录。

该服务器将过滤以.gif，.jpg或.png结尾的请求，并将它们映射到/ data / images目录（通过将URI添加到root指令的参数中），并将所有其他请求传递给上面配置的代理服务器。

## 设置FastCGI 代理

nginx可用于将请求路由到FastCGI服务器，该服务器运行使用各种框架和编程语言（例如PHP）构建的应用程序。

```
server {
    location / {
        fastcgi_pass  localhost:9000;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param QUERY_STRING    $query_string;
    }

    location ~ \.(gif|jpg|png)$ {
        root /data/images;
    }
}
```

这将设置一个服务器，该服务器将通过FastCGI协议将除静态图像请求以外的所有请求路由到在localhost：9000上运行的代理服务器。