# 开发环境

##  vscode智能提示功能
##  调试
###  默认支持node调试，F5启动调试;
### 启动网页调试
 需要安装debugger for chrome, 启动本地服务器:
全局安装http-server,运行命令yarn -g http-server ，http-server启动服务

## 扩展
安装vetur（可以高亮显示代码）和vue vscode snippets（智能快速代码填充）
## 小结
vs快捷生成html: !+enter
vs快速生成vue单文件结构：vbase
输入vfor快速生成循环列表

# Vue设计思想
* 数据驱动应用
* MVVM模式践行者
--------------------------------------------------
![](全栈vue_files/1.png)

# 语法基础
## 插入普通文本和属性绑定：{{}} 和 v-bind（即:）
## 列表渲染
```javascript
<div v-for="c in courses" :key="c">
{{c}}
</div>
```
## 用户输入
## 计算属性和watch
计算属性有缓存性：如果值没有发生变化，则页面不回重新渲染;
默认情况下watch初始化时不执行
## 事件名称用羊肉串方式编写，避免用驼峰式，因为HTML不区分大小写
# 组件
## 组件传值
最简单的方式是通属性（props）传值
子组件向父组件传值，通过自定义事件和事件监听
子组件向父组件派发事件，是通过vue的$emit方法派发
## 组件上使用v-model
v-model默认情况会转化成@input，还表示:value属性值的传递
```javascript
v-model="course" 
//相当于
:value="course" @input="($event){course=$event}"  //$event表示要传递出来的参数
```

## 组件复用
同样的路由重复访问路由时，组件是不会重新created的生命周期，为解决这种情况，使组件重新创建更新，可以写一个关于当前路由$router的自定义
```javascript
watch:{
	$router(){
		imediate:true,
		handler(){
			console.log('test');
		}
	}
}
```

...属性的展开运算符

vue的核心思想是一个驱动的理念