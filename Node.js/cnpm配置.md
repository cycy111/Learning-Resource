## cnpm怎么配置？
有很多方法来配置npm的registry地址，下面根据不同情境列出几种比较常用的方法：

###  方法1、临时使用
npm --registry https://registry.npm.taobao.org install [依赖的名称]

###  方法2、持久使用（慎用）
注：这种方法不建议使用，因为使用这种方式会造成之后都要通过淘宝镜像来获取依赖包，如果是公司内部发布到npm的依赖包，会出现下载失败的情况

npm config set registry https://registry.npm.taobao.org

检查是否配置成功

npm config get registry

###  方法3、安装cnpm（推荐）
推荐这种方式是因为既不会影响npm命令，又不用每次都写淘宝地址进行依赖包的安装

npm install -g cnpm --registry=https://registry.npm.taobao.org

## cnpm 怎么用？
###  安装模块
从[registry.npm.taobao.org](https://registry.npm.taobao.org/) 安装所有模块. 当安装的时候发现安装的模块还没有同步过来, 淘宝 NPM 会自动在后台进行同步, 并且会让你从官方 NPM[registry.npmjs.org](https://registry.npmjs.org/)进行安装. 下次你再安装这个模块的时候, 就会直接从 淘宝 NPM 安装了.

$ cnpm install [name]

### 同步模块
直接通过 sync 命令马上同步一个模块, 只有 cnpm 命令行才有此功能:

    $ cnpm sync connect
当然, 你可以直接通过 web 方式来同步: /sync/connect

    $ open https://npm.taobao.org/sync/connect