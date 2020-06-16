---
title: 关于Node.js中require模块需知
date: 2020-06-16 16:52:40
tags:
   - node
---

>原文： https://www.freecodecamp.org/news/requiring-modules-in-node-js-everything-you-need-to-know-e7fbd119be8/

Node 使用两大核心模块来处理模块依赖

- require 模块。

  可以全局使用，没有必要`require('require')`.

- module 模块。

  也可全局直接使用，没必要`require('module')`

你可以将require模块当作一个命令，将module模块当作所有引用模块的组织者。

require 模块在node中并不是一个复杂的概念。

`const config = require('/path/to/file');`

暴露给require模块的主对象是一个函数（正如上例）。当node 引用require()函数，将本地路径作为函数的唯一参数，node实际上进行了以下步骤

- Resovling，解析。找到文件的绝对路径
- Loading，加载。确定文件内容的类型。
- Warpping，打包。给文件一个私有作用域。这就为什么我们的require模块和module模块对象能被任何本地文件使用。只要我们引用了它。
- Evaluating，评估。这是沙箱环境对加载的代码做的事情。
- Caching：缓存。所以当我们再次引用这个文件，不再需要把所有的步骤再走一次。

这篇文章将会用例子解释不同阶段，他们是如何影响我们在node中写 模块的方式。

让我首先在终端中创建一个目录，来管理这些例子：`mkdir ~/learn-node && cd ~/learn-node`

剩余文章的所有命令，将从``~/learn-node`开始执行。

## Resolving a local path

先介绍module对象，你也可以在一个简单的[REPL](https://www.tutorialspoint.com/nodejs/nodejs_repl_terminal.htm)中查看。

```
~/learn-node $ node
> module
Module {
  id: '<repl>',
  exports: {},
  parent: undefined,
  filename: null,
  loaded: false,
  children: [],
  paths: [ ... ] }
```

每个模块都有一个id属性来标识他。这个id通常是文件的全路径，在 REPL 部分，就是`<repl>`。

node模块在文件系统中有一对一的关系。我们通过将一个文件的内容加载到内存中来引用一个模块。

然而，由于node允许多种方式来引用一个文件（如，一个相对的路径，或者一个与预定义的路径），在我们将文件的内容载入内存之前，我们需求找到他的绝对路径。

当我们引用一个'find-me'模块，没有具体路径：`require('find-me');`

这时，node会查找`find-me.js`，且根据modle.paths里面规定路径挨个查找。

```
~/learn-node $ node
> module.paths
[ '/Users/samer/learn-node/repl/node_modules',
  '/Users/samer/learn-node/node_modules',
  '/Users/samer/node_modules',
  '/Users/node_modules',
  '/node_modules',
  '/Users/samer/.node_modules',
  '/Users/samer/.node_libraries',
  '/usr/local/Cellar/node/7.7.1/lib/node' ]
```

这个路径名单是每个目录下的node_modules 目录，从当前的目录到根目录下目录名单。其中也包括一些不推荐使用的遗留目录。

如果node在任何这些目录中都没有找到`find-me.js`，就会抛出错误`"cannot find module error."`

```
~/learn-node $ node
> require('find-me')
Error: Cannot find module 'find-me'
    at Function.Module._resolveFilename (module.js:470:15)
    at Function.Module._load (module.js:418:25)
    at Module.require (module.js:498:17)
    at require (internal/module.js:20:19)
    at repl:1:1
    at ContextifyScript.Script.runInThisContext (vm.js:23:33)
    at REPLServer.defaultEval (repl.js:336:29)
    at bound (domain.js:280:14)
    at REPLServer.runBound [as eval] (domain.js:293:12)
    at REPLServer.onLine (repl.js:533:10)
```

如果你创建一个本地`node_modules`目录并且放入`find-me.js`文件，那么`require('find-me')`代码就能找到这个文件。

```
~/learn-node $ mkdir node_modules 

~/learn-node $ echo "console.log('I am not lost');" > node_modules/find-me.js

~/learn-node $ node
> require('find-me');
I am not lost
{}
>
```

本地和根目路都有find-me.js文件的话，根目录下的文件不会被加载。
如果删掉本地，就会加载根目录下的find-me.js文件。

## Requiring a folder

模块不一定得是一个文件。我们也可以在`node_modules`下创建find-me 文件夹，在文件夹下放入index.js文件。我们代码`require('find-me')`同样会读取文件下的index.js 文件。

```
~/learn-node $ mkdir -p node_modules/find-me

~/learn-node $ echo "console.log('Found again.');" > node_modules/find-me/index.js

~/learn-node $ node
> require('find-me');
Found again.
{}
>
```

注意：因为我们本地有了这个文件，所以他再次忽略了根目录中的`node_modules`

我们引用一个文件夹，会默认引用其下的index.js文件。当然我们也可以在package.json设定main属性来确定文件夹下从哪个文件开始引用。

### require.resolve

如果你想引用模块却不执行，那你可以使用`node_modules` 函数，他表现起来几乎跟require函数一样，却不加载。如果文件不存在，他依然会报错，如果找到了会返回全路径。

```
> require.resolve('find-me');
'/Users/samer/learn-node/node_modules/find-me/start.js'
> require.resolve('not-there');
Error: Cannot find module 'not-there'
    at Function.Module._resolveFilename (module.js:470:15)
    at Function.resolve (internal/module.js:27:19)
    at repl:1:9
    at ContextifyScript.Script.runInThisContext (vm.js:23:33)
    at REPLServer.defaultEval (repl.js:336:29)
    at bound (domain.js:280:14)
    at REPLServer.runBound [as eval] (domain.js:293:12)
    at REPLServer.onLine (repl.js:533:10)
    at emitOne (events.js:101:20)
    at REPLServer.emit (events.js:191:7)
>
```

这可用来，检查是个可选包是否被安装了，或者在其可用时只使用他。

## Relative and absolute paths

相对路径和绝对路径。

我们除了在`node_modules`下引用模块，我们还可以将模块放在任何位置，用相对路径（`./` 、` ../`)或者绝对路径( `/`)来引用。

例如，我们的`find-me.js`文件在lib文件夹下而不是`node_modules`下，那么我们这样引用：

> ```js
> require('./lib/find-me');
> ```

## Prarent-child relation between files

```
~/learn-node $ node index.js
In index Module {
  id: '.',
  exports: {},
  parent: null,
  filename: '/Users/samer/learn-node/index.js',
  loaded: false,
  children: [],
  paths: [ ... ] }
In util Module {
  id: '/Users/samer/learn-node/lib/util.js',
  exports: {},
  parent:
   Module {
     id: '.',
     exports: {},
     parent: null,
     filename: '/Users/samer/learn-node/index.js',
     loaded: false,
     children: [ [Circular] ],
     paths: [...] },
  filename: '/Users/samer/learn-node/lib/util.js',
  loaded: false,
  children: [],
  paths: [...] }
 
```

- 为避免循环用。父文件的子文件，只用[Circular] 表示。
- 循环依赖在node 中是允许存在的。

### 模块的一些概念

为了更好理解上述。得理解一些module模块的其他概念。

1.在任何模块中，exports 是一个特殊的对象。

我们可以把exports对象的属性替换。但却不适合将exports整个替换。

因为每个模块内的exports实际上指代module.exports，他管理着所有导出的属性。如果我们重新给对象赋值，意味引用丢失，重新赋了一个变量，而不是修改原有对象。

2.关于loaded 属性

模块使用这个属性来跟在哪个模块被加载。

当我们的node完成加载，exoprots对象也就完成。整个过程（引用，加载）是同步的。这也是为什么能够看到模块能够完全加载，在一个事件循环之后。

这也意味着我们不能异步改变exports的对象。

### 循环模块的依赖

让我们来尝试回答node中循环引用的重要问题。当模块1引用模块2，模块2引用模块1会发生什么？

以下两个文件互相依赖：

```
// lib/module1.js

exports.a = 1;

require('./module2');

exports.b = 2;
exports.c = 3;

// lib/module2.js

const Module1 = require('./module1');
console.log('Module1 is partially loaded here', Module1);
```

运行module1 ，发现如下：

```
~/learn-node $ node lib/module1.js
Module1 is partially loaded here { a: 1 }
```

至少能运行。

在模块1完全加载前，我们引用了模块2，而模块2没有完全加载时又引用了模块1.我们当前点，能从exoport对象上得到的所有在循环引用前导出的属性。而b和c 是循环点之后导出的，所以不能打印。

node将这一点做的非常简单，在加载模块的时候，他建立exports对象，你可以在完成加载器引用他，然后你会得到一个部分的导出对象，任何之前你定义导出内容

### JSON和C/C++ 插件

我们可以本地引用一个js文件，通过 在本地引用JSON文件和C++ 插件文件。你甚至都不需要定义一个文件扩展名来做这件事。

如果一个文件无扩展名，第一件事情，node会尝试引用一个js文件。如果没有找到，会尝试找.json 文件，如果确定是一个JSON文本会解析他。这之后他会尝试找到一个二进制的node文件。然而，为了避免歧义，在引用任何其他文件的时候，最好加上扩展，如`.js`。

引用JSON文件的时候，如果json文件里面管理的是静态的配置值，或者你临时从外部读取的资源。比如

```
{
  "host": "localhost",
  "port": 8080
}
```

那么可以直接引用

```
const { host, port } = require('./config');

console.log(`Server will run at http://${host}:${port}`);
```

如果node没有发现`.js`文件或者`.json`文件，他会寻找`.node`文件。解析文件为编译的插件模块。

官方有一个用c++ 写的模板插件文件。用了他你也可以像解析其他模块一样解析node文件了。

## All code you write in Node will be wrapped in functions

node的模块打包通常被误解，为了更好理解，来提醒一下exports`/`module.exports他们之间的关系。

我们可以使用exports对象来导出属性，但是我们不能直接替换exports，因为他知识module.exports的一个引用。

```
exports.id = 42; // This is ok.

exports = { id: 42 }; // This will not work.

module.exports = { id: 42 }; // This is ok.
```

为什么这个exoports导出对象，看上去似乎是所有对象的全局对象，却被定义为module对象的一个引用呢。

在解释node打包过程之前，我先问一个问题：在浏览器中，我们在脚本中这样定义一个变量：`var answer = 42;`

这个变量是在所有脚本里都可全局使用，只要用了定义之后。

在node里却不是这样。在编译一个模块之前，node将模块编码打包到一个函数中，我们可以观察一下模块的warpper属性：

```
~ $ node
> require('module').wrapper
[ '(function (exports, require, module, __filename, __dirname) { ',
  '\n});' ]
>
```

node不会直接执行你文件中的任何代码，他执行打包函数将你的代码放入他的体内。这就是能将定义的顶层变量局限在任何一个当前模块中的原因。

该打包函数有5个参数

`exports`, `require`, `module`, `__filename`, and `__dirname`.

这就是为什么他们看上去像全局的，实际是定义在各自的模块中。

所有的这个参数会在node执行打包函数的时候，拿到他们的值。最先的是exports定义为module.exports的引用。require和module都明确了函数的执行。而`__filename`/__dirname 变量包含打包模块的绝对名字和目录路径。

既然模块都被包裹在了一个函数里面，我们实际上可用参数关键字来获得函数的变量。

## The require object

require 对象没有任何特殊的。他仅仅是一个对象表现得像一个函数，以模块名或者路径作参，返回一个`module.exports`对象。如果我们愿意，我们完全可以用自己的逻辑重写require 对象。



## All modules will be cached

node会缓存第一次调用

```
require('./ascii-art') // will show the header.
require('./ascii-art') // will not show the header.
```

在第一次引用的时候，我们可以将`require.cache` 打印出来。我们可以将他的一个属性删除来是来缓存能力失效。这样node每次从新加载模块都会重新缓存。

然后，这不是最高效的方法。最简单的方式是用一个函数包裹，并立即执行。这样每次执行也会重新缓存。

```
require('./ascii-art')() // will show the header.
require('./ascii-art')() // will also show the header.
```

参考：

1.手写 CommonJS 中的 require函数: https://juejin.im/post/5dcff7fcf265da0baa463aab#heading-12

