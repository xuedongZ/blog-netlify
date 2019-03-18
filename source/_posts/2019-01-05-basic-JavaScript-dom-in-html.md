---
title: HTML中关于JavaScript和dom的基础知识
date: 2019-01-05 16:04:25
tags:
  - JavaScript
---



## JavaScript的工作方式

1.编写

编写HTML标记和JavaScript代码，并把他们放在文件中，比如说index.html和index.js （或者也可以都放在HTML文件中）

2.加载

浏览器获取并加载你的页面，从上到下解析你的内容。
遇到JavaScript时，浏览器会解析代码，检查它的正确性，然后执行代码。
浏览器还会建立HTML页面的一个内部模型，称为DOM。

3.运行

JavaScript继续执行，使用DOM检查页面、完成修改、从页面接受事件，或者要求浏览器从web服务器获取其他数据。

## JavaScript能做什么？
一旦有一个包含`<script>`元素的页面（或者包含引用，指向一个单独的JavaScript文件），你就已经在开始编写代码了。JavaScript是一个完备的编程语言，用其他语言能做到的事情用JavaScript同样能做到，甚至还能做的更多，因为我们就在web页面内部编程！

我们可以用JavaScript：
- 建立一个语句（语句）
- 重复做事情（循环）
- 做出判断（条件）
- 声明变量，赋值变量      
  
> 命名变量要符合三个要求：   
1.变量要以一个字母、下划线或者美元符开头。   
2.然后可以使用任意多个字母、数字、下划线或美元符。   
3.一定要避开JavaScript的所有保留字。

- 构建表达式

  JavaScrit中表达式无处，不在一定要知道可以表达哪些内容：

  

  1).数值表达式

  ```
  (9/5) * tempC + 32;
  x - 1;
  Math.random() * 10;
  2.123 + 3.2;
  ```
  2).布尔表达式

  ```
  2 > 3;
  startTime > now;
  tempF < 75;
  pet = "Duck";
  level == 4;
  ```
  3).串表达式

  ```
  "super" + "cali" + youKnowTheRest;
  "March" + "21" + "st";
  p.innerHTML;
  phoneNumber.substring(0,3);
  ```

  4).其他表达式

  ``` 
  function () {...};
  document.getElementById("pink");
  new Array(10);
  ```

## 在页面中增加JavaScript，怎么加？在那里加？

要使用JavaScript，肯定必须把它增加到一个Web页面中。不过在哪里加呢？另外怎么加？我们已经知道有一个`<script>`元素，所以下面来看在哪里使用这个元素，另外它会如何影响页面中JavaScript的执行。向页面增加代码JavaScript的执行。向页面增加代码有三种不同的方式：

**1.联机放在`<head>`元素中。**

向页面增加代码时，最常见的方式就是在`<head>`部分放置一个`<script>`元素。在`<head>`元素中增加JavaScript代码时，一旦浏览器开始解析head部分就会执行这个代码（这会最先执行），然后才解析页面的其余部分。

**2.通过引用一个单独的JavaScript文件来增加脚本。**

还可以链接到一个包含JavaScript代码的单独文件。将这个文件的URL放在开始`<script>`标记的src属性中，另外一定要用`</script>`结束这个脚本元素。如果链接到同一目录中的某个文件，可以只使用该文件名（无需提供完整的路径）。

```
<head>
<script src="mycode.js"></script>
</head>
```

**3.将代码增加到文档体中，可以作为内联代码，也可以作为一个单独文件链接。**

同样有可用`<script>`直接引用或间接引用。

## JavaScript如何与页面交互

1.在浏览器中加载一个页面时，浏览器和解析 HTML，并创建文档的一个内部模型其中包含HTML标记中的所有元素。

![我们把他简称为DOM](https://ws4.sinaimg.cn/large/006tNc79gy1fywx8ral7uj31a80t4wfj.jpg)

2.JavaScript可以与DOM交互来访问元素以及元素中的内容。Javascript还可以使用DOM来创建或删除元素（当然还可以做很多其他事情）



3.JavaScript修改了DOM时，浏览器会动态更新页面，所以你会看到页面上的新内容。

document表示浏览器中的整个页面，它包含完整的DOM，所以可以让它做任何事情。

- 比如查找有一个特定id的元素。

  `var planet = document.getElementById("greenplanet")`;

  比如替换该id的内容。

  `planet.innerHTML = "Red Alert: hit by phaser fire!";`

- 页面完全加载前不要打扰DOM

  ``` 
  <script>
  function init() {
      var planet = document.getElementById("greenplanet");
      planet.innerHTML = "Red Alert: hit by phaser fire!";
  }
  windown.onload = init
  </script>
  ```

- 向DOM创建或增加元素

- 从DOM删除元素

- 获取和设置元素的属性



  总之，HTML的DNA由描述性标记构成，允许你描述构成页面的一组嵌套元素。另一面，JavaScript却是由纯算法性基因物质构成，主要用来描述计算。

  虽然他们的基因有天壤之别，好在他们有共同之处：DOM。于是通过DOM，JavaScript就能和页面互相通信。而且通信的方法有很多种。而如今各种前端框架的盛行，就是事先搭建，包装好了底层的dom操作，让工程师更多关于业务逻辑。代码的工程繁琐度也再不断降低。
