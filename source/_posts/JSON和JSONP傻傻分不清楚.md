---
title: JSON和JSONP傻傻分不清楚
date: 2019-1-14 21:57:55
tags:
  - JavaScript
---

其实json和jsonp本质上都是字符串，只是jsonp是外层包裹了一层函数。

## JSON
JSON是JavaScript对象记法（JavaScript Object Notation），它有很多好处，规模、可读性等，还有另外一点：他是Web上最流行语言（JavaScript）的内置记法。JSON 是存储和交换文本信息的语法，类似 XML。但JSON 比 XML 更小、更快，更易解析。

### XML让位，JSON登场
曾几何时，我们都把XML当作救星，这是一种人类可读、机器可解析的数据格式，也是原本要支持世界上所有数据需求的数据格式。刚开始开发XMLhttpRequest时，XML确实是大家交互数据采用的方法。
XML 和所谓 HTML 的写法非常相似。XML 没有预定义的标签。在 HTML 中使用的标签（以及 HTML 的结构）是预定义的。HTML 文档只使用在 HTML 标准中定义过的标签。XML 允许创作者定义自己的标签和自己的文档结构。

下面有这样一个资料：
> 一个『名字』叫做多拉Ａ梦，身高『129.3』，体重『129.3』，喜欢的事物有吃『铜锣烧』和『机油』，以及『和小咪约会』的机器人。

【用XML方式存储】：
```
<robot>
	<name>多拉A梦</name>
	<height>129.3</height>
	<weight>129.3</weight>
	<hobby>
		<food>铜锣烧、机油</food>
		<relationship>和小咪约会</relationship>
	</hobby>
</robot>
```

不过这个世界日新月异，如今我们可以获取各种各样的数据格式(CSV、Excel、XML、JSON、yaml)。目前数据交互更多的是JSON请求。
JSON 是存储和交换文本信息的语法。类似 XML。JSON 比 XML 更小、更快，更易解析。
【用JSON方式存储】：

```
{
	“name” : “多拉A梦”,
	“height”: 129.3,
	“weight”:129.3,
	“hobby”:{
		“food” : “铜锣烧、机油”,
		“relationship” : “和小咪约会”
	}
}
```

是不是易读多了呢。
### JSON语法
JSON所表示的数据要么就是对象，要么就是数组。
[从结构上看，所有的数据（data）最终都可以分解成三种类型](http://www.ruanyifeng.com/blog/2009/05/data_types_and_json.html)：

- 第一种类型是标量（scalar），也就是一个单独的字符串（string）或数字（numbers），比如"北京"这个单独的词。

- 第二种类型是序列（sequence），也就是若干个相关的数据按照一定顺序并列在一起，又叫做数组（array）或列表（List），比如"北京，上海"。

- 第三种类型是映射（mapping），也就是一个名/值对（Name/value），即数据有一个名称，还有一个与之相对应的值，这又称作散列（hash）或字典（dictionary），比如"首都：北京"。

数据构成的最小单位原来如此简单！难怪在编程语言中，只要有了数组（array）和对象（object）就能够储存一切数据了。
JSON语法是javaScript语法的子集，javaScript用[]中括号来表示数组，用{}大括号来表示对象，JSON亦是如此。

【JSON数组】：
```
var employees = [
    { "firstName":"Bill" , "lastName":"Gates" },
    { "firstName":"George" , "lastName":"Bush" },
    { "firstName":"Thomas" , "lastName": "Carter" }
];
```
【JSON对象】：
```
var obj = {

    age: 20,
    str: "xiaoming",
    method: function () {
        alert("我爱学习");
    }
};
```
当然啦，数组可以包含对象，在对象中也可以包含数组。
### JSON的规则
JSON能够以非常简单的方式来描述数据结构，XML能做的它都能做，因此在跨平台方面两者完全不分伯仲。

1、JSON只有两种数据类型描述符，大括号{}和方括号[]，其余英文冒号:是映射符，英文逗号,是分隔符，英文双引号""是定义符。

2、大括号{}用来描述一组“不同类型的无序键值对集合”（每个键值对可以理解为OOP的属性描述），方括号[]用来描述一组“相同类型的有序数据集合”（可对应OOP的数组）。

3、上述两种集合中若有多个子项，则通过英文逗号,进行分隔。

4、键值对以英文冒号:进行分隔，并且建议键名都加上英文双引号""，以便于不同语言的解析。

5、JSON内部常用数据类型无非就是字符串、数字、布尔、日期、null 这么几个，字符串必须用双引号引起来，其余的都不用，日期类型比较特殊，这里就不展开讲述了，只是建议如果客户端没有按日期排序功能需求的话，那么把日期时间直接作为字符串传递就好，可以省去很多麻烦。


## JSONP
JSONP是服务器与客户端跨源通信的常用方法。最大特点就是简单适用，老式浏览器全部支持，服务器改造非常小。

它的基本思想是，网页通过添加一个`<script>`元素，向服务器请求JSON数据，这种做法不受同源政策限制；服务器收到请求后，将数据放在一个指定名字的回调函数里传回来。

首先，网页动态插入`<script>`元素，由它向跨源网址发出请求。

```
function addScriptTag(src) {
  var script = document.createElement('script');
  script.setAttribute("type","text/javascript");
  script.src = src;
  document.body.appendChild(script);
}

window.onload = function () {
  addScriptTag('http://example.com/ip?callback=foo');
}

function foo(data) {
  console.log('Your public IP address is: ' + data.ip);
};
```
上面代码通过动态添加`<script>`元素，向服务器example.com发出请求。注意，该请求的查询字符串有一个callback参数，用来指定回调函数的名字，这对于JSONP是必需的。

服务器收到这个请求以后，会将数据放在回调函数的参数位置返回。

```
foo({
  "ip": "8.8.8.8"
});
```
由于`<script>`元素请求的脚本，直接作为代码运行。这时，只要浏览器定义了foo函数，该函数就会立即调用。作为参数的JSON数据被视为JavaScript对象，而不是字符串，因此避免了使用JSON.parse的步骤。

以上。

---
参考文章：
1.[【原创】说说JSON和JSONP，也许你会豁然开朗，含jQuery用例](https://www.cnblogs.com/dowinning/archive/2012/04/19/json-jsonp-jquery.html)
2.[基本資料格式: XML 和 JSON](https://ithelp.ithome.com.tw/articles/10203632?sc=iThelpR)
3.[面试问题之：JSON是什么？](https://blog.csdn.net/u011240877/article/details/46651249)
4.[JSON与JS对象的区别](https://blog.csdn.net/Yeoman92/article/details/54924930)
5.[浏览器同源政策及其规避方法](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)
