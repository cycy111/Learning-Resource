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
```javascript
function Person(name) {
    if (!new.target) {
        throw "must use new operator with Person";
    }
    this.name = name;
}
```
#### JavaScript new.target in constructors
``` javascript
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
Symbol()函数接受一个可选参数作为描述,可以通过toString()方法访问symbol的描述属性:
```javascript
let s = Symbol('foo');
let s = new Symbol(); // error
console.log(Symbol() === Symbol()); // false
let firstName = Symbol('first name'),
	lastName = Symbol('last name');
console.log(firstName); // Symbol(first name)
console.log(lastName); // Symbol(last name)
```

	### Sharing symbols
全局符号注册表允许全局共享symbol, 使用Symbol.for()方法可以创建一个共享的symbol
```javascript
let ssn = Symbol.for('ssn');
let citizenID = Symbol.for('ssn');
console.log(ssn === citizenID); // true
let systemID = Symbol('sys');
console.log(Symbol.keyFor(systemID)); // undefined
```
### 使用Symbol
#### 使用它作为唯一值
```javascript
let statuses = {
    OPEN: Symbol('Open'),
    IN_PROGRESS: Symbol('In progress'),
    COMPLETED: Symbol('Completed'),
    HOLD: Symbol('On hold'),
    CANCELED: Symbol('Canceled')
};
// complete a task
task.setStatus(statuses.COMPLETED);
```
#### 使用它作为对象的可计算属性
```javascript
let status = Symbol('status');
let task = {
    [status]: statuses.OPEN,
    description: 'Learn ES6 Symbol'
};
console.log(task);
```
使用Object.keys()获取一个对象中所有可遍历的属性：
```javascript
console.log(Object.keys(task)); // ["description"]
```
使用Object.getOwnPropertyNames()获取对象中所有的属性：
```javascript
console.log(Object.getOwnPropertyNames(task)); // ["description"]
```
使用Object.getOwnPropertySymbols()获取对象中所有属性symbols:
```javascript
console.log(Object.getOwnPropertySymbols(task)); //[Symbol(status)]
```
### Well-known symbols
Well-known symbols即ES6中预定义的symbols, Well-known symbols表示JavaScript中公共的行为。每个Well-known symbols都是
静态属性
#### Symbol.hasInstance
Symbol.hasInstance是一个可以改变instanceof操作符的行为的symbol
```javascript
//instanceof的一般用法
obj instanceof type;
```
JavaScript调用Symbol.hasIntance方法
```javascript
type[Symbol.hasInstance](obj);
```

```JavaScript
class Stack {
}
console.log([] instanceof Stack); // false
```
想让[]数组成为Stack的实例，可以使用Symbol.hasIntance方法：
```javascript
class Stack {
    static [Symbol.hasInstance](obj) {
        return Array.isArray(obj);
    }
}
console.log([] instanceof Stack); // true
```
#### Symbol.iterator
通过System.iterator symbol访问默认的遍历对象：
```javascript
var iterator = numbers[Symbol.iterator]();

console.log(iterator.next()); // Object {value: 1, done: false}
console.log(iterator.next()); // Object {value: 2, done: false}
console.log(iterator.next()); // Object {value: 3, done: false}
console.log(iterator.next()); // Object {value: undefined, done: true}
```
默认情况下，一个集合不是可遍历的，可以这样是其可遍历：
```javascript
class List {
    constructor() {
        this.elements = [];
    }

    add(element) {
        this.elements.push(element);
        return this;
    }

    *[Symbol.iterator]() {
        for (let element of this.elements) {
            yield  element;
        }
    }
}

let chars = new List();
chars.add('A')
     .add('B')
     .add('C');

// because of the Symbol.iterator
for (let c of chars) {
    console.log(c);
}

// A
// B
// C
```
#### Symbol.isConcatSpreadable
ES6之前：
```javascript
let list = {
    0: 'JavaScript',
    1: 'Symbol',
    length: 2
};
let message = ['Learning'].concat(list);
console.log(message); // ["Learning", Object]
```
ES6:
```javascript
let list = {
    0: 'JavaScript',
    1: 'Symbol',
    length: 2,
    [Symbol.isConcatSpreadable]: true
};
let message = ['Learning'].concat(list);
console.log(message); // ["Learning", "JavaScript", "Symbol"]
```
#### Symbol.toPrimitive
javascript引擎在美国标准类型的原型上定义Symbol.toPrimitive方法。
Symbol.toPrimitive方法默认接收hint（隐式）参数， 这个参数是三个值：“number”, “string”, and “default”其中的一个。
hint参数配置返回值的类型。hint参数被JavaScript引擎基于被使用的对象的上下文填充。
```JavaScript
function Money(amount, currency) {
    this.amount = amount;
    this.currency = currency;
}
Money.prototype[Symbol.toPrimitive] = function(hint) {
    var result;
    switch (hint) {
        case 'string':
            result = this.amount + this.currency;
            break;
        case 'number':
            result = this.amount;
            break;
        case 'default':
            result = this.amount + this.currency;
            break;
    }
    return result;
}

var price = new Money(799, 'USD');

console.log('Price is ' + price); // Price is 799USD
console.log(+price + 1); // 800
console.log(String(price)); // 799USD
```
# ITERATORS & GENERATORS
## Iterators
ES6新引进的循环结构：**for...of**
遍历ranks数组：
```javascript
let ranks = ['A', 'B', 'C'];
//ES6之前
for (let i = 0; i < ranks.length; i++) {
    console.log(ranks[i]);
}
//ES6
for(let rank of ranks) {
    console.log(rank);
}
```

Iteration protocols:  iterable protocol and iterator protocol.
### Iterator protocol
一个对象完成了带有如下问题的接口，是一个iterator：
* 是否还剩下其他元素
* 如果还有元素，那这个元素是什么

从技术的角度来说， 如果一个对象有一个返回两个属性（done和value）的next（）方法，被认为是一个iterator
```javascript
//调用next()返回next值
{ value: 'next value', done: false }

//如果最后一个值返回后再去调用next（）方法将会返回
{done: true: value: undefined}
```
### Iterable protocol
如果对象包含一个被叫做[Symbol.iterator]的方法，这个方法不带参数，并且返回一个符合iterator protocol的对象，我们称这个对象是可遍历的。
创建对象：
```javascript
class Sequence {
    constructor( start = 0, end = Infinity, interval = 1 ) {
        this.start = start;
        this.end = end;
        this.interval = interval;
    }
    [Symbol.iterator]() {
        let counter = 0;
        let nextIndex = this.start;
        return  {
            next: () => {
                if ( nextIndex <= this.end ) {
                    let result = { value: nextIndex,  done: false }
                    nextIndex += this.interval;
                    counter++;
                    return result;
                }
                return { value: counter, done: true };
            }
        }
    }
};
```
使用Sequence iterator ：
```javascript
let evenNumbers = new Sequence(2, 10, 2);

for (const num of evenNumbers) {
    console.log(num);
}
```
### Cleaning up
[Symbol.iterator]()除了返回next（），可能也会返回retrun()方法。 return（）方法在循环提早结束时被调用.
```javascript
class Sequence {
    constructor( start = 0, end = Infinity, interval = 1 ) {
        this.start = start;
        this.end = end;
        this.interval = interval;
    }
    [Symbol.iterator]() {
        let counter = 0;
        let nextIndex = this.start;
        return  {
            next: () => {
                if ( nextIndex <= this.end ) {
                    let result = { value: nextIndex,  done: false }
                    nextIndex += this.interval;
                    counter++;
                    return result;
                }
                return { value: counter, done: true };
            },
            return: () => {
                console.log('cleaning up...');
                return { value: undefined, done: true };
            }
        }
    }
}
```

```javascript
let oddNumbers = new Sequence(1, 10, 2);

for (const num of oddNumbers) {
    if( num > 7 ) {
        break;
    }
    console.log(num);
}

```
输出结果：
```javascript
1
3
5
7
cleaning up...
```
## 生成器函数（Generators）
一般的函数在执行过程中是不中断的，ES6中引入了和一般函数不一样的新的函数：function generator or generator.
一个generator 可以在中途中断，然后再从中断的位置继续执行：
```javascript
function* generate() {
    console.log('invoked 1st time');
    yield 1;
    console.log('invoked 2nd time');
    yield 2;
}
```
* function前面的*号表示generate() 是一个generator
* yield语句返回一个值，并且中断函数的执行

下面调用generate()：
```javascript
let gen = generate();
console.log(gen);
```
输出：
```javascript
Object [Generator] {}
```
一个generator 返回的是一个Generator 对象,它调用的时候没有执行函数体。
Generator 对象返回的是一个有两个属性（value，done）的对象,也就是说Generator 是可遍历的对象。
调用next（）：
```javascript
let result = gen.next();
console.log(result);
```
输出：
```javascript
invoked 1st time
{ value: 1, done: false }
```
### 实例
1. 生成永无止境的序列:
```javascript
function* forever() {
    let index = 0;
    while (true) {
        yield index++;
    }
}
let f = forever();
console.log(f.next()); // 0
console.log(f.next()); // 1
console.log(f.next()); // 2
```
2. 使用generators完成iterators
```javascript
class Sequence {
    constructor( start = 0, end = Infinity, interval = 1 ) {
        this.start = start;
        this.end = end;
        this.interval = interval;
    }
    * [Symbol.iterator]() {
        for( let index = this.start; index <= this.end; index += this.interval ) {
            yield index;
        }
    }
}
```
生成奇数序列：
```javascript
let oddNumbers = new Sequence(1, 10, 2);
for (const num of oddNumbers) {
    console.log(num);
}
```
输出：
```javascript
1
3
5
7
9
```
## yield
### 介绍
yield 的语法：
```javascript
[variable_name] = yield [expression];
```
### 使用实例
*   Returning a value
```javascript
function* foo() { 
    yield 1;
    yield 2;
    yield 3;
}
let f = foo();
console.log(f.next());
```
输出：
```
{ value: 1, done: false }
```

*   Returning undefined
```javascript
function* bar() {
    yield;
}
let b = bar();
console.log(b.next());
```

输出：
```
{ value: undefined, done: false }
```

*  Passing a value to the next() method

```javascript
function* generate() {
    let result = yield;
    console.log(`result is ${result}`);
}

let g = generate();
console.log(g.next()); 

console.log(g.next(1000));
```

输出：
```javascript
{ value: undefined, done: false }
result is 1000
{ value: undefined, done: true }
```

* Using yield in an array
```javascript
function* baz() {
    let arr = [yield, yield];
    console.log(arr);
}
var z = baz();
console.log(z.next());  
console.log(z.next(1)); 
console.log(z.next(2));
```

* Using yield to return an array
```javascript
function* yieldArray() {
    yield 1;
    yield [ 20, 30, 40 ];
}
let y = yieldArray();
console.log(y.next()); 
console.log(y.next()); 
console.log(y.next());
```

* Using the yield to return individual elements of an array
```javascript
function* yieldArrayElements() {
    yield 1;
    yield* [ 20, 30, 40 ];
}
let a = yieldArrayElements();
console.log(a.next()); // { value: 1, done: false }
console.log(a.next()); // { value: 20, done: false }
console.log(a.next()); // { value: 30, done: false }
console.log(a.next()); // { value: 40, done: false }
```
yield *表达式用于委派给另一个可迭代的对象或生成器。
# Promises
## Promises
一个promise 是你希望未来会返回一个值的对象,不是现在返回.
一个promise 有三个状态:pending（进行中）、fulfilled（已成功）和rejected（已失败）
* Pending
* Fulfilled
* Rejected
一个promise从pending状态开始, 这个时候表示promise还没有完成, 以fulfilled (successful) or rejected (failed) 状态结束.
### 创建promise:  the Promise constructor
```javascript
let completed = true;
let learnJS = new Promise(function (resolve, reject) {
    if (completed) {
        resolve("I have completed learning JS.");
    } else {
        reject("I haven't completed learning JS yet.");
    }
});
```
Promise constructor 接收一个函数作为参数, 这个函数叫做executor. 按照规定, executor两个函数的名称: resolve() and reject().
调用new Promise(executor)时,  executor自动被调用.
为了看到promise的pending状态,可以在executor里面使用setTimeout()函数:
```javascript
let completed = true;
let learnJS = new Promise(function (resolve, reject) {
    setTimeout(() => {
        if (completed) {
            resolve("I have completed learning JS.");
        } else {
            reject("I haven't completed learning JS yet.");
        }
    }, 3 * 1000);
});
```
如果promise达到了实现状态或拒绝状态，则promise得以resolved。一旦创建promise, 在resolved之前它都是pending状态.
当promise resolved or rejected, 计划回调,可以使用Promise对象的方法:then(), catch(), and finally().
### Consuming a Promise: then, catch, finally
#### then方法
then带有两个回调函数:
```javascript
promiseObject.then(onFulfilled, onRejected);
```
如果实现了promise，则会调用onFulfilled回调。 当promise被拒绝时，将调用onRejected回调。
#### catch()方法
当promise被拒绝计划执行一个回调,可以使用promise对象的catch()方法
#### finally()方法
无论promise是fullfilled还是rejected时, 都执行的一样的代码
## Promise Chaining
Promise对象的实例方法（比如then(), catch(), or finally()）返回的是一个独立的promise对象，这个对象又可以再次调用
promise的实例方法
## Promise.all()
语法:
```
Promise.all(iterable);
```
iterable 参数是一个promises 列表,作为可遍历对象传入Promise.all().
当您要汇总来自多个异步操作的结果时，Promise.all（）很有用。
### Resolved promises实例
```javascript
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The first promise has resolved');

        resolve(10);
    }, 1 * 1000);
});
const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The second promise has resolved');
        resolve(20);
    }, 2 * 1000);
});
const p3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The third promise has resolved');
        resolve(30);
    }, 3 * 1000);
});
```
想要等这三个promise完成,使用Promise.all()方法:
```javascript
Promise.all([p1, p2, p3])
    .then(results => {
        const total = results.reduce((p, c) => p + c);
        console.log(`Results: ${results}`);
        console.log(`Total: ${total}`);
    });
```
输出结果:
```
The first promise has resolved
The second promise has resolved
The third promise has resolved
Results: 10,20,30
Total: 60
```
### Rejected promises 实例
```javascript
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The first promise has resolved');
        resolve(10);
    }, 1 * 1000);
});
const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The second promise has rejected');
        reject('Failed');
    }, 2 * 1000);
});
const p3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The third promise has resolved');
        resolve(30);
    }, 3 * 1000);
});
Promise.all([p1, p2, p3])
    .then(console.log) // never execute
    .catch(console.log);
```
输出结果:
```
The first promise has resolved
The second promise has rejected
Failed
The third promise has resolved
```
## Promise.race()
Promise.race()静态方法,接收一个promises列表,返回一个promise,这个promise是这些promises中只要其中一个promise完成(fullfill)或者拒绝(reject)就返回值或者原因.
语法:
```
Promise.race(iterable)
```
## Promise Error Handling
# 集合
## Map
ES6之前,键值对应使用object, 一个键对应任何数据类型的值.
使用object作为map有些副作用:
* 一个对象有默认的prototype键
* 对象的键必须是string或者symbol,不能使用对象作为键
* 对象没有一个表示map大小的属性
map语法:
```javascript
let map = new Map([iterable]);
```
iterable是包含键值对元素的可迭代的对象
## Set
ES6提供了一个名为Set的新类型，该类型存储任何类型的唯一值的集合。
语法:
```javascript
let setObject = new Set();
//也可以传一个可迭代的对象到set的构造函数中
let setObject = new Set(iterableObject);
```
# ARRAY 扩展
## Array.of()