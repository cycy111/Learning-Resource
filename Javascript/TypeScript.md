# TypeScript开发环境安装

## 安装node.js

使用命令node -v确认是否安装成功

## 安装TypeScript compiler

`npm install -g typescript`

安装完毕后，用`tsc --v`确认是否安装成功

全局安装ts-node 

`npm install -g ts-node`

## 安装VS Code

安装live server插件

Live Server –允许您使用热重载功能启动本地开发服务器。

# TypeScript的类型

* Object types 
* 原始类型

## 原始类型

1. string
2. number
3. boolean
4. null
5. undefined
6. symbol

## string Type

TypeScript使用双引号或者单引号包裹字符串字面量。

也支持模板字符串，它使用（ `）包裹字符串。模板字符串可以创建多行字符串，并提供字符串插值特性。

字符串插值允许将变量嵌入到字符串中，如下所示：

```
let firstName: string = `John`;
let title: string = `Web Developer`;
let profile: string = `I'm ${firstName}. 
I'm a ${title}`;

console.log(profile);
```

## Object types

对象类型包含方法，数组，类等等



