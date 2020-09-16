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
