## 访问Dom

在DOM中的每个元素都有对应的JavaScript对象。DOM提供很多访问这些对象的方式：

document.getElementByTagName()

document.getElementByClassName()

document.getElementById()

document.querySelector(): 返回在DOM中与给定的选择器中匹配的第一个元素

document.querySelectorAll()

DOM是树状结构，DOM提供很多方法和属性来使用树结构浏览、选择其中的元素：

*element*.children

*element*.parentElement

element.nextElementSibling + element.previousElementSibling

## 修改DOM

访问DOM元素可以修改他们的内容，大多数情况只需要两个属性：innerHTML 和 textContent

## 修改class

使用元素样式和属性，使用classList属性

```
myElement.classList.add(className);
header.style.padding = '1em';
setAttribute()//设置属性
```

## 创建DOM

使用document.createElement

```
let newArticle = document.createElement('article');.
newArticle.appendChild(newHeading);
```

## 替换和移除DOM元素

```
replaceChild()` and  `removeChild()
```

