---
title: Typescript 入门
date: 2020-06-27 22:50:40
tags:
    - Typescript
---

JavaScript 由于历史原因以及各种机缘巧合，成为了目前使用最广泛的前端开发语言，但是也还是因为一些历史原因，在语言设计上同时有很多的一些不足，特别是现代应用工程越来越庞大和复杂，参与的协同人员也越来越多，JavaScript 受限于本身的一些特性（动态弱类型），使得针对工程化项目的开发有些难以应付。广大的开发者制定了各种规范和工具来弥补这些不足，比如 ESLint，不过它远远不能满足我们的要求，并不是每个团队都会使用，总的来说，约束力不够（仅仅是代码风格，而不不含变量类型检查），如果能够直接从语言层面上来解决这个问题是一个不错的方式，于是TypeScript出现了。


## 为什么使用TS

我们写程序过程中，是会出错，这点毋庸置疑。小项目凭看或者运行调试，也能定位到错误。而JavaScript作为动态类型语言，意味者程序运行期间才会做类型检查。

即使检查出了错误，你还要区分错误类型：是计算错误，还是调用一个不存在的方法（不同类型的数据有不同的操作方式或方法，如：字符串类型的数据就不应该直接参与数学运算），又或者是其他什么原因。
其实归结JS动态语言特性造成的，而静态语言这方面就无困扰。

**静态语言的优势**

- 程序编译阶段（配合IDE、编辑器甚至可以在编码阶段）即可发现一些潜在错误，避免程序在生产环境运行了以后再出现错误
- 编码规范、有利于团队开发协作、也更有利于大型项目开发、项目重构
- 配合IDE、编辑器提供更强大的代码智能提示/检查
- 代码即文档

那我们JavaScript能否也吸收这样的优势呢。于是出现了Typescript。让JavaScript虽不是静态语言，但也可以做静态类型检查，结合IDE工具在编写阶段，就"将错误扼杀在摇篮里''。

基本不用写typeof，在写代码的时候就知道哪个地方出错。只要提前写好type，interface，d.ts等约定类型配置或声明文件，就再也不用检查拿到的数据是否是string、number、undefined、null了。

### Typescript特点

- typescript是JavaScript的超集。意味者具备JavaScript基本语法规则，还自带其他一些独有特性。当然最重要的就是他的类型检测和对ES6的支持。
- 类型系统实际上就是最好的文档，大部分函数看类型定义就知道如何使用，而传统的js代码，只能通过console.log打印参数和值来判断一个陌生函数的作用。
- TypeScript 增加了代码的可读性和可维护性。很适合用来构建大型应用程序。
- typescirpt可以通过自身编译器或者babel 编译成纯净、简洁的js代码。并且可以运行在任何浏览器上、Node.js 环境中和任何支持 ECMAScript 3（或更高版本）的JavaScript 引擎中。



## 体验

### 环境搭建

`TypeScript` 编写的程序并不能直接通过浏览器运行，我们需要先通过 `TypeScript` 编译器把 `TypeScript` 代码编译成 `JavaScript` 代码。

而`TypeScript` 的编译器是基于 `Node.js` 的，所以我们需要先安装 `Node.js`

**安装nodejs**

在node[官网](https://nodejs.org/)按介绍安装。安装完成以后，可以通过 `终端` 或者 `cmd` 等命令行工具来调用 `node`

````
node -v
````

 **安装 `TypeScript` 编译器**

```shell
npm i -g typescript
```

**安装完成以后，我们可以通过命令 `tsc` 来调用编译器**

```shell
# 查看当前 tsc 编译器版本
tsc -v        
```



### 编写代码

默认情况下，`TypeScript` 的文件的后缀为 `.ts`

在hello.ts文件中如下编写

```
// ./src/hello.ts
let str: string = 'ts代码';
```

### 编译执行

使用我们安装的 `TypeScript` 编译器 `tsc` 对 `.ts` 文件进行编译。

```
tsc ./src/hello.ts
```

默认情况下会在当前文件所在目录下生成同名的 `js` 文件

为了生成的js文件为我们想要的效果，还需做些编译配置。

### **一些有用的编译选项**

**--outDir**

指定编译文件输出目录。

````shell
tsc --outDir ./dist ./src/hello.ts
````

**--target**

指定编译的代码版本目标，默认为 `ES3`

```shell
tsc --outDir ./dist --target ES6 ./src/hello.ts
```

**--watch**

在监听模式下运行，当文件发生改变的时候自动编译

```sh
tsc --outDir ./dist --target ES6 --watch ./src/hello.ts
```

### 编译配置文件

我们可以把编译的一些选项保存在一个指定的 `json` 文件中，默认情况下 `tsc` 命令运行的时候会自动去加载运行命令所在的目录下的 `tsconfig.json` 文件，配置文件格式如下

```json
{
	"compilerOptions": {
		"outDir": "./dist",
		"target": "ES2015",
    "watch": true,
	},
  // ** : 所有目录（包括子目录）
  // * : 所有文件，也可以指定类型 *.ts
  "include": ["./src/**/*"]
}
```

有了单独的配置文件，我们就可以直接运行: `tsc`

**指定加载的配置文件**

使用 `--project` 或 `-p` 指定配置文件目录，会默认加载该目录下的 `tsconfig.json` 文件

```shell
tsc -p ./configs
```

也可以指定某个具体的配置文件

```
tsc -p ./configs/ts.json
```

## 应用

ts最基础的应用即类型标注：就是在代码中给数据（变量、函数（参数、返回值））添加类型说明，当一个变量或者函数（参数）等被标注以后就不能存储或传入与标注类型不符合的类型

有了标注，`TypeScript` 编译器就能按照标注对这些数据进行类型合法检测。

有了标注，各种编辑器、IDE等就能进行智能提示。

在 `TypeScript` 中，类型标注的基本语法格式为：

```typescript
数据载体:类型
```

`TypeScript` 的类型标注，我们可以分为

 

### 简单的类型标注

#### 1.基础类型：string，number，boolean

```
let title: string = 'kkb';
let n: number = 100;
let isOk: boolean = true;
```

#### 2.空和未定义类型

因为在 `Null` 和 `Undefined` 这两种类型有且只有一个值，在标注一个变量为 `Null` 和 `Undefined` 类型，那就表示该变量不能修改了。

```
let a: null;
// ok
a = null;
// error
a = 1;
```

默认情况下 `null` 和 `undefined` 是所有类型的子类型。 就是说你可以把 `null` 和 `undefined` 其它类型的变量。

```
let a: number;
// ok
a = null;
```

**小技巧：**

因为 `null` 和 `undefined` 都是其它类型的子类型，所以默认情况下会有一些隐藏的问题

```typescript
let a:number;
a = null;
// ok（实际运行是有问题的）
a.toFixed(1);
```

> 小技巧：指定 `strictNullChecks` 配置为 `true`，可以有效的检测 `null` 或者 `undefined`，避免很多常见问题

#### 3.对象类型

**1.内置对象类型**

在 `JavaScript` 中，有许多的内置对象，比如：Object、Array、Date……，我们可以通过对象的 `构造函数` 或者 `类` 来进行标注

```typescript
let a: object = {};
// 数组这里标注格式有点不太一样，后面我们在数组标注中进行详细讲解
let arr: Array<number> = [1,2,3];
let d1: Date = new Date();
```

**2.自定义对象类型**

另外一种情况，许多时候，我们可能需要自定义结构的对象。这个时候，我们可以：

- 字面量标注

```
let a: {username: string; age: number} = {
  username: 'zMouse',
  age: 35
};
// ok
a.username;
a.age;
// error
a.gender;
优点 : 方便、直接
缺点 : 不利于复用和维护
```


- 接口

```
interface Person {
  username: string;
  age: number;
};
let a: Person = {
  username: 'zMouse',
  age: 35
};

优点 : 复用性高
缺点 : 接口只能作为类型标注使用，不能作为具体值，它只是一种抽象的结构定义，并不是实体，没有具体功能实现
```



- 定义 类 或者 构造函数

```
interface AjaxOptions {
    url: string;
    method: string;
}

function ajax(options: AjaxOptions) {}

ajax({
    url: '',
    method: 'get'
});
```

```
class Person {
	constructor(public username: string, public age: number) {
  }
}
// ok
a.username;
a.age;
// error
a.gender;

优点 : 功能相对强大，定义实体的同时也定义了对应的类型
缺点 : 复杂，比如只想约束某个函数接收的参数结构，没有必要去定一个类，使用接口会更加简单

```

#### 4.数组类型

1.简单标注

```
let list: number[] = [1, 2, 3];
```

2.范型标注

```
let list: Array<number> = [1, 2, 3];
```



#### 5.元组类型

元组类似数组，但是存储的元素类型不必相同，但是需要注意：

- 初始化数据的个数以及对应位置标注类型必须一致
- 越界数据必须是元组标注中的类型之一（标注越界数据可以不用对应顺序）

```
let x: [string, number];
// Initialize it
x = ["hello", 10]; // OK
// Initialize it incorrectly
x = [10, "hello"]; // Error
```

#### 6.枚举类型

枚举的作用组织收集一组关联数据的方式，通过枚举我们可以给一组有关联意义的数据赋予一些友好的名字。

注意事项：

- key 不能是数字
- value 可以是数字，称为 数字类型枚举，也可以是字符串，称为 字符串类型枚举，但不能是其它值，默认为数字：0
- 枚举值可以省略，如果省略，则：
  - 第一个枚举值默认为：0
  - 非第一个枚举值为上一个数字枚举值 + 1
- 枚举值为只读（常量），初始化后不可修改

#### 7.无值类型

表示没有任何数据的类型，通常用于标注无返回值函数的返回值类型，函数默认标注类型为：`void`

```typescript
function fn():void {
  	// 没有 return 或者 return undefined
}
```

#### 8.Never 类型

当一个函数永远不可能执行 `return` 的时候，返回的就是 `never` ，与 void 不同，`void` 是执行了 `return`， 只是没有值，`never` 是不会执行 `return`，比如抛出错误，导致函数终止执行

```typescript
function fn(): never {
  	throw new Error('error');
}
```

#### 9.任意类型

有的时候，我们并不确定这个值到底是什么类型或者不需要对该值进行类型检测，就可以标注为 `any` 类型

```typescript
let a: any;
```

- 一个变量申明未赋值且未标注类型的情况下，默认为 `any` 类型
- 任何类型值都可以赋值给 `any` 类型
- `any` 类型也可以赋值给任意类型
- `any` 类型有任意属性和方法

注意：标注为 `any` 类型，也意味着放弃对该值的类型检测，同时放弃 IDE 的智能提示

> 小技巧：当指定 `noImplicitAny` 配置为 `true`，当函数参数出现隐含的 `any` 类型时报错

#### 10.未知类型

unknow，3.0 版本中新增，属于安全版的 any，但是与 any 不同的是：

- unknow 仅能赋值给 unknow、any
- unknow 没有任何属性和方法

#### 11.函数类型

在 JavaScript 函数是非常重要的，在 TypeScript 也是如此。同样的，函数也有自己的类型标注格式

- 参数
- 返回值

```typescript
函数名称( 参数1: 类型, 参数2: 类型... ): 返回值类型;
function add(x: number, y: number): number {
  	return x + y;
}
```



### 高级的类型标注

#### 1.联合类型

联合类型也可以称为多选类型，当我们希望标注一个变量为多个类型之一时可以选择联合类型标注，或 的关系

```typescript
function css(ele: Element, attr: string, value: string|number) {
    // ...
}

let box = document.querySelector('.box');
// document.querySelector 方法返回值就是一个联合类型
if (box) {
    // ts 会提示有 null 的可能性，加上判断更严谨
    css(box, 'width', '100px');
    css(box, 'opacity', 1);
    css(box, 'opacity', [1,2]);  // 错误
}
```

#### 2.交叉类型

交叉类型也可以称为合并类型，可以把多种类型合并到一起成为一种新的类型，并且 的关系

对一个对象进行扩展：

```typescript
interface o1 {x: number, y: string};
interface o2 {z: number};

let o: o1 & o2 = Object.assign({}, {x:1,y:'2'}, {z: 100});
```

**小技巧**

> `TypeScript` 在编译过程中只会转换语法（比如扩展运算符，箭头函数等语法进行转换，对于 `API` 是不会进行转换的（也没必要转换，而是引入一些扩展库进行处理的），如果我们的代码中使用了 `target` 中没有的 `API` ，则需要手动进行引入，默认情况下 `TypeScript` 会根据 `target` 载入核心的类型库
>
> ```
> target` 为 `es5` 时: `["dom", "es5", "scripthost"]
> target` 为 `es6` 时: `["dom", "es6", "dom.iterable", "scripthost"]
> ```
>
> 如果代码中使用了这些默认载入库以外的代码，则可以通过 `lib` 选项来进行设置
>
> http://www.typescriptlang.org/docs/handbook/compiler-options.html

#### 3.字面量类型

有的时候，我们希望标注的不是某个类型，而是一个固定值，就可以使用字面量类型，配合联合类型会更有用

```typescript
function setPosition(ele: Element, direction: 'left' | 'top' | 'right' | 'bottom') {
  	// ...
}

// ok
box && setDirection(box, 'bottom');
// error
box && setDirection(box, 'hehe');
```

#### 4.类型别名

有的时候类型标注比较复杂，这个时候我们可以类型标注起一个相对简单的名字

```typescript
type dir = 'left' | 'top' | 'right' | 'bottom';
function setPosition(ele: Element, direction: dir) {
  	// ...
}
```

**使用类型别名定义函数类型**

这里需要注意一下，如果使用 `type` 来定义函数类型，和接口有点不太相同

```typescript
type callback = (a: string) => string;
let fn: callback = function(a) {};

// 或者直接
let fn: (a: string) => string = function(a) {}
```

> **interface 与 type 的区别**
**interface**
- 只能描述 `object`/`class`/`function` 的类型
- 同名 `interface` 自动合并，利于扩展
**type**
- 不能重名
- 能描述所有数据


#### 5.类型推导

每次都显式标注类型会比较麻烦，TypeScript 提供了一种更加方便的特性：类型推导。TypeScript 编译器会根据当前上下文自动的推导出对应的类型标注，这个过程发生在：

- 初始化变量
- 设置函数默认参数值
- 返回函数值

```typescript
// 自动推断 x 为 number
let x = 1;
// 不能将类型“"a"”分配给类型“number”
x = 'a';

// 函数参数类型、函数返回值会根据对应的默认值和返回值进行自动推断
function fn(a = 1) {return a * a}
```

#### 6.类型断言

有的时候，我们可能标注一个更加精确的类型（缩小类型标注范围，因为我们比ts更加清楚实际使用的类型），比如：

```typescript
let img = document.querySelector('#img');
```

我们可以看到 img 的类型为 Element，而 Element 类型其实只是元素类型的通用类型，如果我们去访问 src 这个属性是有问题的，我们需要把它的类型标注得更为精确：HTMLImageElement 类型，这个时候，我们就可以使用类型断言，它类似于一种 类型转换：

```typescript
let img = <HTMLImageElement>document.querySelector('#img');
```

或者

```typescript
let img = document.querySelector('#img') as HTMLImageElement;
```

> 注意：断言只是一种预判，并不会数据本身产生实际的作用，即：类似转换，但并非真的转换了

### 其他

类型标注中用会用到接口，范型，还有一种拓展功能--装饰器。装饰器是一种可以在不修改类代码的基础上通过添加标注的方式来对类型进行扩展的一种方式。

装饰器只能用在类中。装饰器其实在es6中已有该特性，并不是typescript独有，本质是一个函数，具体使用中体会。

## 总结

`Typescript` 是弥补JavaScript动态弱类型特性在某些场景下短板，助力前端项目工程化的一个有力工具。

很香。

---

参考：
1.官网：https://www.typescriptlang.org/
2.TypeScript入门教程：https://ts.xcatliu.com/introduction/index.html