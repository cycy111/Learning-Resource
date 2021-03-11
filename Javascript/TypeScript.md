#  TypeScript是什么

TypeScript向JavaScript添加类型，以在运行JavaScript代码之前捕获错误，从而帮助您加快开发速度。TypeScript是一种基于JavaScript的开源编程语言。 它可以在任何运行JavaScript的浏览器，任何操作系统，任何环境下运行。

TypeScript建立在JavaScript之上。

![img](D:\File\Learning-Resource\images\what-is-typescript-compiler.png)

TypeScript主要的目标是：

* 引入可选类型到JavaScript

* 完成未来JavaScript计划的新特性

  

1) TypeScript在帮助您避免错误的同时提高了生产率

```
function add(x: number, y: number) {
   return x + y;
}
```

通过使用类型，您可以在编译时捕获错误，而不是在运行时发现它们。

2) TypeScript将未来的JavaScript带到今天

TypeScript支持ES Next中为当前JavaScript引擎计划的即将推出的功能。

每年，TC39都会发布ECMAScript的许多新功能，这是JavaScript的标准。 功能建议通常经历五个阶段：

# TypeScript开发环境安装

* Node.js – Node.js是您将在其上运行TypeScript编译器的环境
* TypeScript compiler – 一个将TypeScript编译为JavaScript的Node.js模块。如果将JavaScript用于node.js，则可以安装ts-node模块。 它是Node.js的TypeScript执行和REPL。
* Visual Studio Code or VS code – 支持TypeScript的代码编辑器想·

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

# TS实例

使用TypeScript开发Hello World程序

1. 创建app.ts文件

```
let message: string = 'Hello, World!';
console.log(message);
```

2. 在终端上编译ts文件, 将生成一个新文件app.js

   ```
   tsc app.ts
   ```

3. 在node.js上运行app.js

   ```
   node app.js
   ```

    如果安装了ts-node模块，可以只使用一个命令来编译TypeScript文件并一次执行输出文件：

   ```
   ts-node app.ts
   ```

   

   

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



