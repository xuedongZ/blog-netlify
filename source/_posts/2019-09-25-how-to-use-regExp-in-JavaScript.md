---
title: 正则表达式入门
date: 2019-09-25 14:30:41
tags:
  - JavaScript
---

正则表达式，初一看，这几个字很神秘啊…不明觉厉。话说回来，这也不能怪咱们，如果当初把他翻译成”规则表达式”，很可能初学者不会感到那么大的压力 ，明明每个字都认识。似乎有点故弄玄虚呢，直接看英文`Regular Expression`，就不会有这个字面障碍了。

还有另一障碍，某些文化环境中，人易受标签式思想影响，为一个横空出世、各种加持的概念而激动，顾不上去了解名词背后的真实意思，甚至没有意识到：先看看到底是什么意思，再跟着欣喜也不迟。

我也不例外…曾经也是对正则表达式敬而远之，道听途说地觉得这是高阶才需要掌握的内容。后来遇到问题，走了弯路，还是要还债的。总之一句话，冷静才能看清真相。

我们想象这样一个场景：

> 当你扑哧扑哧，用各种判断、逻辑处理，自认完备地进行各种操作的时候，人家一行代码，早就搞定了。

当这种事情发生的时候，内心波澜不惊，肯定是骗人的。这种”一拳超人”的画面，作为一个有追求的 programmer，能不向往吗？正所谓"侠之大者，一刀结束战斗...”，然后去做更多重要的事儿，这怎么想都相当地划算啊。

既然，正则表达式如此好用，不妨研究一下，为前端工程之路减少障碍。

## wiki

wiki 百科的介绍：

> 正则表达式（英语：Regular Expression，在代码中常简写为 regex、regexp 或 RE），又称正规表示式、正规表示法、正规运算式、规则运算式、常规表示法，是计算机科学的一个概念。正则表达式使用单个字符串来描述、匹配一系列符合某个句法规则的字符串。在很多文本编辑器里，正则表达式通常被用来检索、替换那些符合某个模式的文本。许多程序设计语言都支持利用正则表达式进行字符串操作。例如，在 Perl 中就内建了一个功能强大的正则表达式引擎。正则表达式这个概念最初是由 Unix 中的工具软件（例如 sed 和 grep）普及开的。

今天我们要讲的是 JavaScript 的正则表达式的使用。

## 小案例

需求：从下列字符串中找到所有数字，放入数组中。
let str = "fdsafdafd1231dafdas4354fdsafdsa321";
法一：条件判断

```
function getNumber(str){
    let arr = [];
    let temp = "";
    for(let i =0;i<str.length;i++){
        // 判断是否是数字
        if(!isNaN(str[i])){
            // 是数字；
            temp += str[i];
            // arr.push(str[i]);
        }else{
            if(temp !== ""){
                arr.push(parseInt(temp));
                temp = "";
            }
        }
    }
    if(temp !==""){
        arr.push(parseInt(temp) );
    }
    return arr;
}
console.log(getNumber(str));
```

法二：正则

```
let reg = /\d+/g;
let arr = str.match(reg);
console.log(arr);
```

是不是很简洁。看不懂没关系，我们先熟悉基本用法即可。

## 基本用法

### 正则表达式的创建

1.字面量创建
需求：把 is 找出来

```
let str = "this is a book";
let reg = /\bis\b/g;
let res = str.match(reg);
```

回过头来看，

- 格式是两个斜杠，内加匹配规则。
- g，表示贪婪模式。亦即，把全部符合要求的都找出来，而不是只找第一个。
- 我们要找内容是 is 字符串
- is 字符串的特点有边界的。

  - 而边界又是什么？只要是非”数字、字母、下划线”就是边界。空格算是边界一种。
  - 用`\w`表示”数字、字母、下划线”
  - 用`\b`表示边界。

理清要找字符串"is"的特性，再把他匹配出来就很容易了。

2.构造函数创建
需求：把 is 找出来

```
let str = "this is a book";
let matchstr = "\\bis\\b";  // 通过这样的方式写，边界符这些特殊字符就要转译一下。
let reg = new RegExp(matchstr,"g");
let res = str.match(reg);
console.log(res);
// [ 'is' ]
```

- 要匹配变量的时候，需要用构造函数的写法。
- match 方法匹配出来的结果 res 是个数组。
- 如果两种方法都可以的话，建议用字面量的写法。用构造函数有开销，性能上会差一些。

### 正则的匹配方法

一、正则的方法
1.test
如果需要知道一个字符串是否与一个正则表达式（RegExp）匹配，可使用 test() 。他返回结果是 true 或者 false。

```
let str = 'hello world!';
let result = /^hello/.test(str);
console.log(result);
// true
```

- 此方法类似字符串的 search。
- 此方法会忽略全局匹配 g。（得到结果要么是有，要么是没有，确实没有全局什么事情。）

2.exec
如果需要获得匹配结果，也可以使用 RegExp.exec() 。

```
let str = "fdsaf212dfds1212f";
let reg = /\d+/;
let res = reg.exec(str);
console.log(res);
// [ '212', index: 5, input: 'fdsaf212dfds1212f', groups: undefined ]
```

此方法，类似字符串的 match 方法。

二、字符串的方法
1.split
以某些内容为界，把字符串拆分到数组中。

```
let str = "fdsafd213fdsaf4324fda";
let arr = str.split(/\d+/);
console.log(arr);
// [ 'fdsafd', 'fdsaf', 'fda' ]
```

2.replace
替换：
一个替换元素只替换成一个`*`

```
let str = "fdsafd213fdsaf4324fda"
let res = str.replace(/\d/g,"*");
console.log(res);

```

一个替换元素（可能有多个字符）替换成多个`*`

```
let str = "fdsafd213fdsaf4324fda"
let res = str.replace(/\d+/g,function(arg){
    // console.log(arg);
    let restr = "";
    for(let i=0;i<arg.length;i++){
        restr +="*";
    }
    return restr;
});
```

实战中，这回调函数可用于敏感词过滤。

3.search
找到第一次匹配的索引值（会忽略全局匹配），如果没有搜索到返回`-1`。

```
let str = "adfsdf234dfafd234";
let reg = /\d+/;
let res = str.search(reg);
console.log(res);
// 6
```

此方法类似 string 的 indexOf()。

4.match
见开头案例。
默认返回第一个符合要求的内容, 结构为数组，可设置为全局匹配模式匹配所有的内容。

## 字符规则

正则表达式，都是由字符构成的，理解各部分字符的含义，就能理解表达式的完整意图。其中元字符比较简单好记，其他字符为了便于理解，我们将其分类。

### 元字符

元字符：正则表达式中拥有特殊含义的字母字符；
`. * + ? $ ^ | \ () [] {};`

`.`：（小数点）默认匹配除换行符之外的任何单个字符。
`*`：有，或者没有，0 个或多个。
`+`：匹配前面一个表达式 1 次或者多次。等价于 {1,}。
`?`：匹配前面一个表达式 0 次或者 1 次。等价于 {0,1}。
`$`：匹配输入的结束。如果多行标示被设置为 true，那么也匹配换行符前的位置。
`^`：匹配输入的开始。如果多行标志被设置为 true，那么也匹配换行符后紧跟的位置。
`|`：或者（在集合中就没有必要使用了）
`\`：用于转译，把特殊含义的字符 转换成字面含义。
`()`：分组
`[]`：集合
`{}`：对前面出现的内容的描述。

### 基础字符

- `.`

  - 匹配行结束符（\n \r \u2028 或 \u2029）以外的任意单个字符
  - 在 `字符集合（Character Sets）` 中，. 将失去其特殊含义，表示的是原始值

- `\`，转义符，它有两层含义

  - 表示下一个具有特殊含义的字符为字面值
  - 表示下一个字符具有特殊含义（转义后的结果是元字符内约定的）

* `\d` 匹配任意一个阿拉伯数字的字符
* `\D` 匹配任意一个非阿拉伯数字的字符
* `\w` 匹配任意一个（字母、数字、下划线）的字符
* `\W` 匹配任意一个非（字母、数字、下划线）的字符
* `\s` 匹配一个空白符，包括空格、制表符、换页符、换行符和其他 Unicode 空格
* `\S` 匹配一个非空白符
* `\t` 匹配一个水平制表符（tab）
* `\r` 匹配一个回车符（carriage return）
* `\n` 匹配一个换行符（linefeed）
* `\v` 匹配一个垂直制表符（vertical tab）
* `\f` 匹配一个换页符（form-feed）

细心看就发现，如果同样的字母有大写，就是`非`的意思。

### 字符集合

`[xyz]`

- 一个字符集合，也叫字符组。匹配集合中的任意一个字符。你可以使用连字符'-'指定一个范围
- `[^xyz]` 是一个反义或补充字符集，也叫反义字符组。也就是说，它匹配任意不在括号内的字符。你也可以通过使用连字符 '-' 指定一个范围内的字符

### 边界

- `^`
  匹配输入开始。如果多行（multiline）标志被设为 true，该字符也会匹配一个断行（line break）符后的开始处
- `\$`
  匹配输入结尾。如果多行（multiline）标志被设为 true，该字符也会匹配一个断行（line break）符的前的结尾处
- `\b`
  匹配一个零宽单词边界（zero-width word boundary）
- `\B`
  匹配一个非零宽单词边界（zero-width word boundary）

### 分组（子项）

- 可以使用 () 对表达式进行分组，类似数学中分组，也称为子项
- 索引分组
- 命名分组
  - (?<name>...)
  - groups 属性
- 捕获匹配
  - 具有捕获（capturing）特性，即会把匹配结果保存到（子项结果）中
  - (x)
- 非捕获匹配
  - 不具有捕获（capturing）特性，即不会把匹配结果保存到（子项结果）中
  - (?:x)
- 零宽断言/预查（Assertions）
  - 用于指定查找在某些内容(但并不包括这些内容)之前或之后的内容
  - 正向零宽断言/预查
    - 肯定
      - (?=pattern)
    - 否定
      - (?!pattern)
  - 负向零宽断言/预查（注意：ES2018 新增）
    - 肯定
      - (?<=pattern)
    - 否定
      - (?<!patten)
- 捕获与零宽断言的区别
  - 捕获：匹配的内容出现在结果中但不出现在子项结果中
  - 零宽断言：完全不会出现在结果

### 反向引用

- `\$n`
  这里的 n 表示的是一个变量，值为一个数字，指向正则表达式中第 n 个括号（从左开始数）中匹配的子字符串

### 数量词汇

- `x{n}`
  n 是一个正整数。前面的模式 x 连续出现 n 次时匹配
- `x{n,m}`
  n 和 m 为正整数。前面的模式 x 连续出现至少 n 次，至多 m 次时匹配
- `x{n,}`
  n 是一个正整数。前面的模式 x 连续出现至少 n 次时匹配
- `x\*`
  匹配前面的模式 x 0 或多次
- `x+`
  匹配前面的模式 x 1 或多次。等价于 {1,}
- `x?`
  匹配前面的模式 x 0 或 1 次
- `x|y`
  匹配 x 或 y

### 匹配模式

- `g`
  global，全局模式：找到所有匹配，而不是在第一个匹配后停止

- `i`
  ignore，忽略大小写模式：匹配不区分大小写

- `m`
  multiple，多行模式：将开始和结束字符（^和\$）视为在多行上工作，而不只是匹配整个输入字符串的最开始和最末尾处

- `s`
  dotAll / singleline 模式：. 可以匹配换行符

- `u`
  unicode，unicode 模式：匹配 unicode 字符集

  ```js
  console.log(/^.$/.test('\uD842\uDFB7'));
  console.log(/^.$/u.test('\uD842\uDFB7'));
  ```

- `y`
  sticky，粘性模式：匹配正则中 lastIndex 属性指定位置的字符，并且如果没有匹配也不尝试从任何后续的索引中进行匹配

## 补充

### 一些常用的等价表述

```
x|y === [xy]  //表示 x或y 任一
? === {0,1}; // 有一个或者没有
. ===> [^\n\r];   //
\d ==>[0-9];  // 数字，0-9
\w ===>[a-zA-Z_0-9];
```

### 部分案例

个别用法可能需要案例辅助理解，下面挑选几个比较陌生的：
**1.反向引用**
需求：转换时间格式，将 2019-9-17 转成格式 17/9/2019;
代码实现如下：

```
let mytime = "2019-10-18";
let reg  = /(\d{4})-(\d{1,2})-(\d{1,2})/g;
let res = mytime.replace(reg,"$3/$2/$1");
console.log(res);
//console.log(RegExp.$1);
// 18/10/2019
```

**2.命名分组**
这是 es2018，即 es9 的特性。

```
let str = "fdsfdsafd23432fdfda2312s";
let reg = /(?<num>\d+)/;
let res = str.match(reg);
console.log(res.groups.num);

```

不在全局模式下，匹配出的内容会有详细信息，这些信息放在一个数组里面（匹配内容，匹配下标，输入值，groups）。

**3.零宽断言**
这也是 es2018 的特性，只讲正向，逆向类似。
3.1 正向肯定（?=）
需求：把其后有数字的「iphone」都替换成苹果

```
let str = "iphone7iphone8iphone11iphonenumber";
let reg = /iphone(?=\d{1,2})/g;
let res = str.replace(reg,"苹果");
console.log(res);
// 苹果7苹果8苹果11iphonenumber
```

3.2 正向否定（?!）
需求：把其后不是数字的「iphone」都替换成苹果

```
let str = "iphone7iphone8iphone11iphonenumber";
let reg = /iphone(?!\d{1,2})/g;
let res = str.replace(reg,"苹果");
console.log(res);
// iphone7iphone8iphone11苹果number
```

## 工具

最后，推荐 2 个 Regex 测试器，

- https://regex101.com/ 这是目前（2019）网上最方便的。
- https://regexper.com/ 对于一个复杂正则表达式，它可以解析成图形，帮助理解。

示范：想知道`/^@[a-zA-Z]d+@$/`在干什么，将其输入，得到解析图形如下:
![](https://raw.githubusercontent.com/xuedongZ/blog-img/img/2019/regImg.png)
是不很直观呢。

---

## 参考

1.MDN 教程，https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions
