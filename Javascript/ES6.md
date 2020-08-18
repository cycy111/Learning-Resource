# ES新语法
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