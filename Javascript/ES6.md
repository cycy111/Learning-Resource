# ES新语法
## arguments 和 parameter区别
## Default Parameters
## 剩余参数(Rest Parameter)
ES6提供了一种新型的参数，即所谓的rest参数，其前缀为三个点（...）。语法如下:
```
function fn(a,b,...args) {
   //...
}
```
最后一个参数就是rest参数. rest参数允许您将无限数量的参数表示为数组。
例如:
```
fn(1,2,3,'A','B','C');
```
args存储的值为:
`[3,'A','B','C']`
--------------------------------------------------
## (扩展运算符)Spread operator 
扩展运算符(spread)是三个点(...)作用:将一个字面量对象(比如an array,a  map, or a set.)转为用逗号分隔的其中的元素。
### Spread operator 和Rest parameter区别
* Spread operator展开所有对象中的元素
* Rest parameter打包元素到数组中
```
const odd = [1,3,5];
const combined = [...odd, 2,4,6];
console.log(combined);
```
输出:
`[ 1, 3, 5, 2, 4, 6 ]`

--------------------------------------------------

## Object Literal Syntax Extensions in ES6
### property initializer shorthand
在ES6之前, 对象字面量是这样的:
```
function createMachine(name, status) {
    return {
        name: name,
        status: status
    };
}
```
ES6是这样:
```
function createMachine(name, status) {
    return {
        name,
        status
    };
}
```
在内部，当对象字面量的属性仅具有名称时，JavaScript引擎将在周围的范围内搜索具有相同名称的变量。如果找到一个，它将为属性分配变量的值。
```
let name = 'Computer',
    status = 'On';

let machine = {
   name,
   status
};
```
### computed properties
```
let prefix = 'machine';
let machine = {
    [prefix + ' name']: 'server',
    [prefix + ' hours']: 10000
};

console.log(machine['machine name']); // server
console.log(machine['machine hours']); // 10000
```
### Concise method syntax
ES6之前,为对象定义方法:
```
let server = {
    name: 'Server',
    restart: function() {
        console.log('The' + this.name + ' is restarting...');
    }
};
```
ES6:
```
let server = {
    name: 'Server',
    restart() {
        console.log(`The ${this.name} is restarting...`);
    }
};
```

--------------------------------------------------
## for…of Loop in ES6
ES6引入了一个新的结构for...of, 能够遍历所有实现iterator protocol的任何对象,包括an Array, a Map, a Set.
for...of语法:
```
for (variable of iterable) {
   // statements 
}
```

--------------------------------------------------
## Octal and binary literals (八进制和二进制文字)
### octal literal
In ES5, to represent an octal literal, you use the zero prefix ( 0) followed by a sequence of octal digits (from 0 to 7).
```
let a = 051;
console.log(a); // 41
```
如果八进制中包含超出范围的数字,前缀将被忽略:
```
let b = 058; // invalid octal
console.log(b); // 58
```
在严格模式下会报错:
`SyntaxError: Decimals with leading zeros are not allowed in strict mode.`

ES6允许您使用前缀0o指定八进制文字:
```
let d = 0o58;
console.log(d); // SyntaxError
```
### Binary literals
在ES5中没有表示二进制的格式, 通常用parseInt()转化:
```
let e = parseInt('111',2);
console.log(e); // 7
```
在ES6中支持使用0b前缀表示二进制:
```
let f = 0b111;
console.log(f); // 7
```

--------------------------------------------------

## 字面量模板(Template Literals)
在ES6之前, 使用单引号或者双引号包装字符串字面量.
在ES6中，通过将文本包装在反引号中来创建模板文字，如下所示：
```
let simple = `This is a template literal`;
```
ES6Z中字面量模板有如下特性:
* 跨多行字符串:一个可以跨越多行的字符串。
* 字符串格式化: 将变量和表达式替换字符串中的一部分, 也叫字符串插值.
* HTML转义：转换字符串的能力，以便可以安全地包含在HTML中。

通过使用反引号，您可以自由使用模板文字中的单引号或双引号，而无需转义。
```
let anotherStr = `Here's a template literal`;
```
如果字符串中包含反引号,需要用反斜杠(\)转义:
```
let strWithBacktick = `Template literals use backticks \` insead of quotes`;
```
字面量模板和普通字符串之间的最大区别是替换(substitutions)。替换允许您将变量和表达式嵌入字符串中。JavaScript引擎将自动用它们的值替换这些变量和表达式。 此过程也称为字符串插值。
### 跨行字符串
### Variable and expression substitutions
要指示JavaScript替换变量和表达式，将变量和表达式放在特殊的块中，如下所示：
```
${variable_name}
```
### Tagged templates

## 解构赋值
## 对象解构
--------------------------------------------------
# ES Modules & Class
## ES6模块
一个ES6模块是一个只在严格模式下执行的js文件, 也就是说声明在模块中的变量和方法不会自动加到全局作用域中.
### 在浏览器中执行模块
首先定义一个message.js文件:
```
export let message = 'ES6 Modules';
```
第二步, 创建一个app.js文件, 在其中引用message.js模块:
```
import { message } from './message.js'

const h1 = document.createElement('h1');
h1.textContent = message

document.body.appendChild(h1)
```

最后, 创建一个html页面,在其中使用app.js模块:
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>ES6 Modules</title>
</head>
<body>
<script type="module" src="./app.js"></script>
</body>
</html>
```
在这里, 我们通过在script标签中使用type="module"来加载app.js模块.

## CLASS基础
ES6之前定义类:
```
function Animal(type) {
    this.type = type;
}

Animal.prototype.identify = function() {
    console.log(this.type);
}

var cat = new Animal('Cat');
cat.identify(); // Cat
```

ES6定义类:
```
class Animal {
    constructor(type) {
        this.type = type;
    }
    identify() {
        console.log(this.type);
    }
}

let cat = new Animal('Cat');
cat.identify();
```

类声明只是构造函数的语法糖:
```
console.log(typeof Animal); // function
```
注意: 语法糖是JavaScript编程语言中的语法，旨在使事情更容易表达和清楚。
### Class vs. custom type
* 类声明不像函数声明那样悬挂。比如把下面这段代码放Animal类声明前就会报错:
```
let dog = new Animal('Dog');
// Uncaught ReferenceError: Animal is not defined
```
* 在严格模式下, 类中的代码会自动执行
* 类中的对象是不可枚举遍历的, 但如果constructor function就需要使用Object.defineProperty()使属性不可枚举遍历
* 类在调用构造函数时,不能缺少new, 不然会报错
```
let duck = Animal('Duck');
// Uncaught TypeError: Class constructor Animal cannot be invoked without 'new'
```
### JavaScript class expressions
```
let Animal = class {
    constructor(type) {
        this.type = type;
    }
    identify() {
        console.log(this.type);
    }
}

let duck = new Animal('Duck');

console.log(duck instanceof Animal); // true
console.log(duck instanceof Object); // true

console.log(typeof Animal); // function
console.log(typeof Animal.prototype); // function
```
### First-class citizen
类可以传入函数, 然后从这个函数里面返回, 再赋值给一个变量:
```
function factory(aClass) {
    return new aClass();
}

let greeting = factory(class {
    sayHi() {
        console.log('Hi');
    }
});

greeting.sayHi(); // 'Hi'
```
### Singleton
使用类创建单例:
```
let app = new class {
    constructor(name) {
        this.name = name;
    }
    start() {
        console.log(`Starting the ${this.name}...`);
    }

}('Awesome App');

app.start(); // Starting the Awesome App...
```
### Getter and setter
```
class Person {
    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
    get fullName() {
        return this.firstName + ' ' + this.lastName;
    }

    set fullName(str) {
        let names = str.split(' ');
        if (names.length === 2) {
            this.firstName = names[0];
            this.lastName = names[1];
        } else {
            throw 'Invalid name format';
        }
    }
}

let mary = new Person('Mary', 'Doe');
console.log(mary.fullName); // Mary Doe

// set new name
mary.fullName = 'Mary William';
console.log(mary.fullName); // Mary William
```
### Computed member names
```
let name = 'fullName';
class Person {
    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
    get[name]() {
        return this.firstName + ' ' + this.lastName;
    }
    set[name](str) {
        //...
    }
}

var john = new Person('John', 'Doe');
console.log(john.fullName); // John Doe
```
### static method
ES6之前:
```
Animal.make = function(type) {
    return new Animal(type);
}

var dog = Animal.make('Dog');
dog.identify(); // Dog
```
ES6:
```
lass Animal {
    constructor(type) {
        this.type = type;
    }
    identify() {
        console.log(this.type);
    }
    static create(type) {
        return new Animal(type);
    }
}

var mouse = Animal.create('Mouse');
mouse.identify(); // mouse
```
### JavaScript Inheritance Using extends & super
### new.target Metaproperty
new.target对于在运行时检查函数是作为函数执行还是作为构造函数执行非常有用。确定从基类内部使用new运算符调用的特定派生类也很方便。
#### JavaScript new.target in functions
为检测方法是不是使用new调用的, 可以使用new.target.
例如, 如果你不想让Person()作为函数被调用,可以这样写:
```
function Person(name) {
    if (!new.target) {
        throw "must use new operator with Person";
    }
    this.name = name;
}
```
#### JavaScript new.target in constructors
```
class Person {
    constructor(name) {
        this.name = name;
        console.log(new.target.name);
    }
}

class Employee extends Person {
    constructor(name, title) {
        super(name);
        this.title = title;
    }
}

let john = new Person('John Doe'); // Person
let lily = new Employee('Lily Bush', 'Programmer'); // Employee
```
## 箭头函数
## Symbol
ES6新增了一个原生类型:Symbol
### Creating symbols
```
let s = Symbol('foo');
let s = new Symbol(); // error
```

```
console.log(Symbol() === Symbol()); // false
```
Symbol()函数接受一个可选参数作为描述,可以通过toString()方法访问symbol的描述属性:
```
let firstName = Symbol('first name'),
    lastName = Symbol('last name');
console.log(firstName); // Symbol(first name)
console.log(lastName); // Symbol(last name)
```
### Sharing symbols
全局符号注册表允许全局共享symbol, 使用Symbol.for()可以创建一个共享的symbol
```
let ssn = Symbol.for('ssn');
let citizenID = Symbol.for('ssn');
console.log(ssn === citizenID); // true

let systemID = Symbol('sys');
console.log(Symbol.keyFor(systemID)); // undefined
```