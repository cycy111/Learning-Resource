## em和rem

CSS支持几种常用的绝对长度单位，最常用、最基础的是像素px. 不常用的是mm,cm,in英寸、Pt(点，印刷术语1/72in)、pc（派卡，印刷术语，12点）。换算规则：1in=25.4mm=2.54cm=6pc=72pt=96px。设计师常用点作为单位，开发人员习惯用像素。因此跟设计师沟通的时候需要作一些换算。

像素是一个误导性的名称，CSS像素并不 严格等于显示器的像素，尤其在高清屏下（视网膜屏）。

### em

em是常见的相对单位，适合基于特定的字号进行排版。在CSS中，1em等于当前元素的字号，其准确值取决于作用的元素。

```
.padded{
	font-size:16px;
	padding:1em; //设置四个内边距为font-size
}
```

当设置padding、height、width、border-radius等属性时，使用em会很方便。当用户改变字体的设置时，这些属性会跟着元素均匀的缩放。

将em应用于不同的元素：

```
<style>
	.box{
		paddiing:1em;
		border-radius:1em;	
	}
	.box-small{
		font-size:12px;
	}
	.box-large{
		font-size:18px;
	}
</style>
<html>
	<span class="box box-small">small</span>
	<span class="box box-large">large</span>
</html>
```



给元素字号font-size声明em时，字号大小会根据继承的字号来计算。

```
<body>
	we love coffe
	<p class="slogan">we love coffee</p>
</body>

.body{
	font-size:16px;
}
.slogan{
	font-size:1.2em; //计算结果为元素继承的字号的1.2倍
}
```

1. em同时用于字号和其他属性

   这时，浏览器先计算字号，然后根据这个计算值去算出其余的属性值。

   ```
   body{
   	font-size:16px;
   }
   .slogan{
   	font-size:1.2em;
   	padding:1.2em;
   }
   ```

2. 字体缩小的问题

   当用em来指定多重嵌套的元素的字号，就会产生意外的结果。

   ```
   body{
   	font-size:16px;
   }
   ul{
   	font-size:.8em;
   }
   <ul>
   	<li>Top level>
   		<ul>
   			<li> sencond level
   			</li>
   		</ul
   	</li>
   </ul>
   ```

   这样，第二层、第三层的字体会逐级缩小，如果指定大于1em的字号，文字会逐渐增大。

   纠正文字缩小的问题:

   ```
   ul ul{
   	font-size:1em;
   }
   
   ```

   em用在内边距、外边距以及元素大小上很好，但用着字号就很复杂。值得庆幸的是，还有更好的选择：rem。

   ### rem

   <html>元素是顶级（根）节点。根节点是其他元素的祖先节点。根节点有一个伪类选择器（:root）,可以用来选择它自己。

   rem是root em的缩写。rem不是相对于当前元素，而是相对于根元素的单位。



## 视口相对单位

em和rem都是相对于font-size定义的，但CSS里不止有这种相对单位。

视口-----浏览器窗口里网页可见部分的边框区域。

* vh: 视口的1/100.
* vm: 视口宽度的1/100.
* vmin: 视口宽、高中较小的一方的1/100.
* vmax: 视口宽、高中较大的一方的1/100.

### 使用vw定义字号

vm用于设置字号，比用vh和vw设置元素的宽和高还要实用。在一个元素加上font-size:2vw, 在一个1200px的桌面显示器上，计算值为24px；在768pz宽的平板上，计算值为15px.   但是在24屏上来说太大了，在ipone 6会缩小到7.5px。如果能够保留这种缩放的能力，但让极端情况缓和一些就更棒了。CSS的calc()函数就可以提供帮助。



### 使用calc()定义字号

```
 :root {
      font-size: calc(0.5em + 1vw);
 }
```

0.5em保证了最小字号，1vw则确保了字体会随着视口缩放。



## 无单位的数值和行高

有些属性允许无单位的值（如line-height、z-index、 font-weigh）。line-height属性比较特殊，它的值既可以有单位也可以无单位。通常我们应该使用无单位的数值指定行高。

```
<body>
    <p class='about-us'>
    	We have built partnerships with small farms around the world to hand-select beans at the peak of season. We then carefully roast in small batches to maximize their potential.
    </p>

</body>
```

给body元素指定一个行高，允许它被其他元素继承。不管在网页设置了什么字号，这种方式都会按照预期显示。

```
body {
line-height: 1.2;
}

.about-us {
font-size: 2em;
}
  
```

如果用有单位的值定义行高，可能会产生意想不到的结果如下图,每行文字会重叠：

```
body {
	line-height: 1.2em;
}

.about-us {
	font-size: 2em;
}
```

## 自定义属性

层叠变量的自定义属性给CSS引进了变量的概念，开启了一种全新的基于上下文的动态样式。你可以声明一个变量，为它赋值，然后在其他地方引用。

除了IE，自定义属性已经得到各大主流浏览器的支持。要了解更新更全的情况，可以在Can I Use网站中检索“CSS Variables”.

```
:root {
	--main-font: Helvetica, Arial, sans-serif;
}
p {
	font-family: var(--main-font);//使用自定义属性
}
```

使用自定义属性定义颜色：

```
:root {
    --main-font: Helvetica, Arial, sans-serif;
    --brand-color: #369;
}

p {
    font-family: var(--main-font);
    color: var(--brand-color);
}
```

var函数接受第二个参数，它指定了备用值。

```
:root {
	--main-font: Helvetica, Arial, sans-serif;
}

p {
	font-family: var(--main-font, sans-serif);
	color: var(--rand-color, blue);//--rand-color未定义，所以使用了备用值
}

```

### 使用JavaScript改变自定义属性

```

    :root {
      font-size: calc(0.5em + 1vw);
      --main-bg: #fff;
      --main-color: #000;
    }

    body {
      font-family: Helvetica, Arial, sans-serif;
    }

    .dark {
      margin-top: 2em;
      padding: 1em;
      background-color: #999;
      --main-bg: #333;
      --main-color: #fff;
    }

    .panel {
      font-size: 1rem;
      padding: 1em;
      border: 1px solid #999;
      border-radius: 0.5em;
      background-color: var(--main-bg);
      color: var(--main-color);
    }

    .panel > h2 {
      margin-top: 0;
      font-size: 0.8em;
      font-weight: bold;
      text-transform: uppercase;
    }

    .panel.large {
      font-size: 1.2em;
    }
```



```
<div class="panel">
    <h2>Single-origin</h2>
    <div class="body">
      We have built partnerships with small farms
      around the world to hand-select beans at the
      peak of season. We then carefully roast in
      small batches to maximize their potential.
    </div>
  </div>

  <aside class="dark">
    <div class="panel">
      <h2>Single-origin</h2>
      <div class="body">
        We have built partnerships with small farms
        around the world to hand-select beans at the
        peak of season. We then carefully roast in
        small batches to maximize their potential.
      </div>
    </div>
  </aside>

  <script type="text/javascript">
  	let rootElement = document.documentElement;
    let styles = getComputedStyle(rootElement);
    let mainColor = styles.getPropertyValue('--main-bg');
    console.log(String(mainColor).trim());

    rootElement = document.documentElement;
    rootElement.style.setProperty('--main-bg', '#cdf');
  </script>
```



## 盒模型

将两列放到合适的位置，首先用浮动，分别设置70%和30%的宽度。当给一个元素设置宽或高的时候，盒模型默认行为,指定的是内容的宽和高.

```
<style>
        .body{
            background-color: #eee;
            font-family: Arial, Helvetica, sans-serif;
        }
        header{
            color:#fff;
            background-color: #0072b0;
            border-radius: .5em;
        }
        main{
            display: block;
        }
        .main{
            float: left;
            width: 70%;
            //box-sizing: border-box;
            border-radius: .5em;
            background-color: lightcoral;
        }
        .siderbar{
            float: left;
            width: 30%;
            //box-sizing: border-box;
            padding: 1.5em;
            border-radius: .5em;
            background-color: lightgreen;
        }
    </style>
    <body>
    <header>
        <h1>Runing club</h1>
    </header>
    <div class="container">
        <main class="main">
            <h2>Come join us!</h2>
            <p>
                the club meets at 6:00am every thursday at the town square.
            </p>
        </main>
        <aside class="siderbar">
            <div></div>
        </aside>
    </div>   
	</body>
```

两列并没有并排出现，而是折行线上。虽然上面两列的宽度设置为 70%和30%，但他们总共占据的宽度超过了可用空间的100%。

![image-20210719225014472](D:\File\Learning-Resource\images\image-20210719225014472.png)

### 调整盒模型

在CSS中可以用box-sizing属性调整盒模型的行为。box-sizing的默认值为content-box。将属性值设置为border-box后，height和width属性会设置内容、内边距以及边框的总和。这样上面两列刚好并排。

![image-20210719225158319](D:\File\Learning-Resource\images\image-20210719225158319.png)

**给两列间加空隙**

```
.main{
    float: left;
    width: 70%;
    box-sizing: border-box;
    border-radius: .5em;
    background-color: lightcoral;
}
.siderbar{
    float: left;
    width: calc(30% - 1.5em);
    margin-left: 1.5em;
    box-sizing: border-box;
    padding: 1.5em;
    border-radius: .5em;
    background-color: lightgreen;
}
```

使用em指定间距，能让代码意图更加明显。

### 元素高度问题

**控制溢出行为**

用overflow属性可以控制溢出行为，该属性值支持4个值：

* visible
* hidden
* scroll
* auto

**百分比高度**

用百分比指定高度是存在问题的。百分比参考的是容器块的大小，但容器的高度通常是由子元素的高度决定的。要想要百分比生效，必须给父元素明确的高度。

比起使用百分比让容器填满屏幕，更好的方式是用视口的相对单位vh。100vh等于视口的高度。还有一个更常用的用法是创造等高列。

1. 等高列

   不明确的高度很难实现等高列。在21世纪初，CSS取代了**HTML表格**成为主要方式。当时表格是实现等高列的唯一方式。现代浏览器支持了**CSS表格**，可以轻松实现等高列，比如IE8+支持display:table, IE10+支持弹性盒子或者Flexbox,都默认支持等高列。

2. CSS表格布局

   用CSS表格布局替代浮动布局。给容器设置display:table, 给列设置display:table-cell.

   ```
   .container{
               display: table;
               width: 100%;
           }
           .main{
               display: table-cell; 
               width: 70%;
               box-sizing: border-box;
               border-radius: .5em;
               background-color: lightcoral;
           }
           .siderbar{
               display: table-cell;   
               width: 30% ;
               margin-left: 1.5em; //外边距不再生效
               box-sizing: border-box;
               padding: 1.5em;
               border-radius: .5em;
               background-color: lightgreen;
           }
   ```

   不像block元素，默认情况下，显示为table的元素不会扩展到100%，因此需要明确指定宽度 。以上代码基本实现需求，就是缺少间隙。因为外边距不会作用于table-cell元素，所以要修改代码，让间隔生效。

   使用border-spacing属性来定义单元格间距。该属性接受两个长度值：水平间距和垂直间距。

   ```
   .container{
               display: table;
               width: 100%;
               border-spacing:1.5em 0;
           }
   ```

   

   border-spacing作用于单元格之间和表格外边缘

   

![image-20210520154741515](D:\File\Learning-Resource\images\image-20210520154741515.png)

只有两列就无法跟头部左右对齐了，可以给整个表格包裹一层新的容器，使用负外边距。

```
.wrap{
            margin-left: -1.5em;
            margin-right: -1.5em;
            
}
 <header>
        <h1>Runing club</h1>
 </header>
 <div class="wrap">
     <div class="container">
         <main class="main">
             <h2>Come join us!</h2>
             <p>
             	the club meets at 6:00am every thursday at the town square.
             </p>
         </main>
         <aside class="siderbar">
         	<div></div>
         </aside>
     </div>
 </div>
```

![image-20210520180841603](D:\File\Learning-Resource\images\image-20210520180841603.png)

3. Flexbox

给容器设置display:flex, 它就变成了一个弹性容器（flex container），子元素默认等高。可以给子元素设置宽度和外边距，尽管加起来可能超过100%，Flexbox也能妥善处理。

```
.container{
            display: flex;
            width: 100%;
            border-spacing: 1.5em;
        }
       
       .main{
            display: table-cell; 
            width: 70%;
            box-sizing: border-box;
            border-radius: .5em;
            background-color: lightcoral;
        }
        .siderbar{
            display: table-cell;   
            width: 30% ;
            margin-left: 1.5em;
            box-sizing: border-box;
            padding: 1.5em;
            border-radius: .5em;
            background-color: lightgreen;
        }
```

**使用min-height和max-height**

这两个属性指定最小或最大值，而不是明确定义高度，这样元素可以在这些界限内自动决定高度。类似的属性还有min-width和max-width.

### 垂直居中问题

vertical-align对块级元素是不生效的，它只会影响行内元素或者table-cell元素。CSS表格布局可以用vertical-align:middle实现垂直居中。

## 外边距折叠

## 容器内元素间距

给侧边栏加上两个跳转到社交媒体页的按钮。

```
<body>
  <header>
    <h1>Franklin Running Club</h1>
  </header>
  <div class="container">
    <main class="main">
      <h2>Come join us!</h2>
      <p>
        The Franklin Running club meets at 6:00pm every Thursday
        at the town square. Runs are three to five miles, at your
        own pace.
      </p>
    </main>
    <aside class="sidebar">
      <a href="/twitter" class="button-link">
        follow us on Twitter
      </a>
      <a href="/facebook" class="button-link">
        like us on Facebook
      </a>
      <a href="/sponsors" class="sponsor-link">
        become a sponsor
      </a>
    </aside>
  </div>

</body>
```

设置按钮元素间距，使用相邻的兄弟组合器（+）选中同一个父元素下紧跟在其他button-link后面的button-link元素。

```

    :root {
      box-sizing: border-box;
    }

    *,
    ::before,
    ::after {
      box-sizing: inherit;
    }

    body {
      background-color: #eee;
      font-family: Helvetica, Arial, sans-serif;
    }

    header {
      padding: 1em 1.5em;
      color: #fff;
      background-color: #0072b0;
      border-radius: .5em;
    }

    .container {
      display: flex;
    }

    .main {
      width: 70%;
      background-color: #fff;
      border-radius: .5em;
    }

    .sidebar {
      width: 30%;
      padding: 1.5em;
      margin-left: 1.5em;
      background-color: #fff;
      border-radius: .5em;
    }

    .button-link {
      display: block;
      padding: .5em;
      color: #fff;
      background-color: #0090C9;
      text-align: center;
      text-decoration: none;
      text-transform: uppercase;
    }

    .button-link + .button-link {
      margin-top: 1.5em;
    }

    .sponsor-link {
      display: block;
      color: #0072b0;
      font-weight: bold;
      text-decoration: none;
    }
```

![image-20210726215811870](D:\File\Learning-Resource\images\image-20210726215811870.png)

侧边栏第二个按钮和底部链接之间缺少间距, 可以使用更通用的解决方案：猫头鹰选择器，调整样式：

```

    :root {
      box-sizing: border-box;
    }

    *,
    ::before,
    ::after {
      box-sizing: inherit;
    }

    body {
      background-color: #eee;
      font-family: Helvetica, Arial, sans-serif;
    }

    body * + * {			//用猫头鹰选择器全局设置上下堆叠的元素的间距
      margin-top: 1.5em;
    }

    header {
      padding: 1em 1.5em;
      color: #fff;
      background-color: #0072b0;
      border-radius: .5em;
    }

    .container {
      display: flex;
    }

    .main {
      width: 70%;
      padding: 1em 1.5em;
      background-color: #fff;
      border-radius: .5em;
    }

    .sidebar {
      width: 30%;
      padding: 1.5em;
      margin-top: 0;  		//新增加
      margin-left: 1.5em;
      background-color: #fff;
      border-radius: .5em;
    }

    .button-link {
      display: block;
      padding: .5em;
      color: #fff;
      background-color: #0090C9;
      text-align: center;
      text-decoration: none;
      text-transform: uppercase;
    }

    .sponsor-link {
      display: block;
      color: #0072b0;
      font-weight: bold;
      text-decoration: none;
    }

```

猫头鹰选择器会选中页面上有着相同级的非第一个子元素。