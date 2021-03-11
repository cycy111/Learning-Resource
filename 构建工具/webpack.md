

webpack执行构建的时候会找webpack.config.js这个配置文件，如果没有找到会走默认配置。

webpack配置就是一个对象。

bundle打包构建的资源文件，一个chunck对应一个bundle

webpack.config.js

```
const path=require('path')
module.exports={
    //项目打包的相对路径，必须是绝对路径
    //context:process.cwd(),
    //执行构建的入口 项目入口,支持字符串，数组，对象
    entry:'./src/index.js',
    //出口
    output:{
        //构建的文件资源放在哪？必须是绝对路径
        path:path.resolve(__dirname,'build'),
        //构建的文件资源叫什么。 无论是多出口还是单出口，都推荐用占位符
        filename:'[name].js'
        //占位符
        //hash 整个项目的hash值，每构建一次就产生一个新的hash值
        //chunkhash  根据不同入口entry进行依赖解析，构建对应的chunkhash,只有entry的模块没有内容变动，对应的chunkhash	不变
        //name
        //id
    },
    //构建模式 none production development
    mode:'',
    //处理不认识的模块
    module:{
    	rules:[
    		{
    			test:/\.css$/,
    			//loader的执行顺序是从后往前的
    			//css-loader 把css模块加入到js模块中
    			//css in js方式
    			//style-loader 从js中提取css的loader，在html中创建style标签，把css内容放在这个style标签里
    			use:["style-loader","css-loader"]
    		}
    	]
    }
}
```

webpack默认只支持json,js文件，加载其他格式需要用loader来处理