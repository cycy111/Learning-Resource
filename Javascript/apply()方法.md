```
const person = {
    firstName: 'John',
    lastName: 'Doe'
}

function greet(greeting, message) {
    return `${greeting} ${this.firstName}. ${message}`;
}

// use the apply() method to call the greet() function with the this set to the person object

let result = greet.apply(person, ['Hello', 'How are you?']);
console.log(result);

//use the call() method, you need to pass the arguments of the greet() function separately

let result = greet.call(person, Hello', 'How are you?');
```


## 使用apply()方法附加一个数组到另外一个数组

```
let arr = [1, 2, 3];
let numbers = [4, 5, 6];

arr.push.apply(arr, numbers);

console.log(arr); 
```

--------------------------------------------------
```
```

[hello](http://baidu.com)


