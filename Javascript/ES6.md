# ES新语法
## arguments 和 parameter区别
## Default Parameters
## Rest Parameters
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

通过使用反引号，您可以自由使用模板文字中的单引号或双引号，而无需转义。
```
let anotherStr = `Here's a template literal`;
```
如果字符串中包含反引号,需要用反斜杠(\)转义:
```
let strWithBacktick = `Template literals use backticks \` insead of quotes`;
```