---
title: 微信小程序入门
tags: 小程序
date: 2019-02-27 20:56:06
---

## 小程序是什么

**作为使用者**
我想应该不用多说了。基本上我们每个人手机里都会装微信，那或多或少我们都开始使用小程序了。比如我喜欢用的有：mobie，回家吃饭，名片全能王。

对于我们普通用户的感受就是：小程序是减少手机 app 数量的，解决生活需求的小工具。

**作为开发者**
张小龙对小程序的定义：

> 小程序是一种不需要下载安装即可使用的应用，它实现了应用“触手可及”的梦想，用户扫一扫或者搜一下即可打开应用。也体现了“用完即走”的理念，用户不用关心是否安装太多应用的问题。应用将无处不在，随时可用，但又无需安装卸载。

微信之父张小龙用略带文艺气息的描述给小程序做了定义。正式发布之后，我们看到的有电商小程序（比如某东），服务型小程序（比如某团），内容型小程序（比如某到），视频类小程序（比如腾讯视频，自家的）。公测时候大家普遍认为小程序只适合做低频类的应用（内容型应用是内测时普遍认为最不适合做成小程序的一类应用），甚至腾讯自家也首发了一款体验非常不错的腾讯视频小程序（这是哪门子用完即走？）。

现实是，开发小程序还是原生 APP，得综合考虑成本、推广、业务丰富度、微信原生结合功能等等因素。甚至有些商业产品本来就希望粘着用户。

于是我们可以看到，官方乖乖修改了小程序的定义：

> 小程序是一种新的开放能力，开发者可以快速地开发一个小程序。小程序可以在微信内被便捷地获取和传播，同时具有出色的使用体验。

这也许是初创团队的福音，可以借助小程序以最小的获客成本来试行产品。再者，原先小程序只向企业、政府、媒体等组织开放，现在也向个人开放啦。普通用户甚至可以用来开发自用的小工具。

## 如何开发小程序

### 学习资源

小程序官方已经给出了[开发文档](https:// developers.weixin.qq.com/miniprogram/dev/index.htm)。不管哪个框架，官方文档总是最新、最可靠的学习资源。

### 条件准备

此处需要留意的是，着手开发小程序需要一些基础知识：HTML，CSS，JavaScript 等，最好先补一下，不然就可能看得云里雾里，浪费时间了。

### 技术选型

对于小程序开发者来说，小程序的开发生态不断地在完善，从最开始的原生框架，到腾讯自己开发类 vue 规范的[wepy](https://tencent.github.io/wepy/)，再到美团开发近 vue 写法的[mpvue](http://mpvue.com/)，再到 O2 实验室刚推出的遵循 React 规范的[Taro](https://taro.aotu.io/)，不断丰富。

所有到现在，有多种开发微信小程序的方式，主要有原生、wepy、mpvue、Taro，4 种框架。（其实还有一种小程序云开发，目前的呼声较小。）

### 开发流程

我们作为入门，先就熟悉先从原生的框架开始学习，大致流程还是相似的。

#### 起步

- 申请帐号
- 安装开发工具
- 新建小程序 or 改样板小程序
- 编译预览

#### 认识项目框架

小程序框架分为视图层和逻辑层。
视图层是有框架所设计的标签语言 WXML（WeiXin Markup language）和用于描述 WXML 组件样式的 WXSS（WeiXin Style Sheets）组成，他们的关系就像 HTML 与 CSS 的关系。WXML 和 WXSS 会被在渲染时会被框架解析为不同端的本地渲染文件，这样保证一套代码能够在多处运行，并且最大化接近原生 APP。渲染原理和 React Native、Weex 十分接近。

逻辑层是一套运行在本地 JavaScript 引擎的 JavaScript 代码，在此基础上框架实现了一套模块化机制，让每个 JS 文件有独立的作用域和模块化的能力，这套模块化机制遵循 CommonJS 规范，熟悉 nodeJS 的开发者应该有一定了解。

小程序整体开发流程非常接近前端 HTML+CSS+JavaScript 的开发模式，与前端开发不同的是，在小程序中没有 DOM 的概念，在本地 JavaScript 引擎中也没有 window、document 等对象，我们不能想当然地通过操作 DOM 来操作页面，小程序中的视图层和逻辑层的交互是通过数据绑定和事件响应实现的，这是一种单向绑定机制。这套机制需要首先将逻辑层和视图层的数据和事件进行绑定，当需要修改页面时，逻辑层只需要调用特定的 setData 方法修改已绑定的数据，这时框架会自动触发 WXML 重新渲染，达到逻辑层对视图层的控制；当框架接收到用户交互操作时，会根据视图层绑定的事件，执行逻辑层对应的事件函数，达到逻辑层对视图层的响应。

#### 认识项目结构

一个完整的小程序中，文件主要分为框架主体文件和页面文件两大类：

- 框架主体文件是系统级别文件，一个项目只有一份，分别是 app.json、app.js 和 app.wxss。他们分别控制小程序整体配置、逻辑和整体样式，小程序启动时只会执行一次。这 3 个文件必须放在项目根目录里，且文件名必须是 app ，其中 app.json 和 app.js 是必须的。
- 一个小程序有一个或多个页面。一个页面三部分组成：界面、配置和逻辑由.wml、.wxss、.js 和 .json 四个文件组成，他们分别控制页面的结构、样式、逻辑和配置，其中.wxml 和 .js 文件是必须的，按框架规定，同一个页面的这 4 个文件必须具有相同的路径和文件名。

#### 编写代码

利用 JavaScript、CSS、以及框架的知识编写代码，一些知识点记录如下：

1.小程序所支持的 CSS 选择器：`类选择器.class` `id选择器#id` `元素选择器element` `伪元素选择器::after` `伪元素选择器::before`，虽然 CSS1、CSS2 和 CSS3 的选择器加起来总计几十种，但在小程序中只能支持以上几种。

2.使用弹性布局：display: flex;
css 样式可以使页面化腐朽为神奇，应多练多用。学习地址：[css](http://www.w3school.com.cn/css/), [css3](http://www.w3school.com.cn/css3/index.asp), [Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html), [Flex 布局教程：实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html), [Grid 布局指南（上）](https://zhuanlan.zhihu.com/p/33030746), [Grid 布局指南（下）](https://zhuanlan.zhihu.com/p/33031255)

4.使用自适应单位：rpx。
使用 rpx 可以使组件自适应屏幕的高度和宽度。小程序编译后，rpx 会做一次 px 换算。换算是以 375 个物理像素为基准，也就是在一个宽度为 375 物理像素的屏幕下，1rpx = 1px。举个例子：iPhone6 屏幕宽度为 375px，共 750 个物理像素，那么 1rpx = 375 / 750 px = 0.5px。推荐用 iPhone 6 的尺寸来做设计图，换算起来比较方便。

5.通过 swiper 组件实现轮播，比如`<swiper indicator-dots=“true” autoplay=“true” interval=“5000”>`，为每隔 5s 换一张图片。

6.image 组件的 4 种缩放方式和 9 种裁剪方式：

- scaletoFill，aspectFit，aspectFill，widthfix；
- top，bottom，center，left，right，top left，top right，bottom left，bottom right。

7.[页面生命周期函数](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/app.html)：

- onload：小程序初始化完成时（全局只触发一次）
- onshow：小程序启动，或从后台进入前台显示时
- onhide：小程序从前台进入后台时
- onError：小程序发生脚本错误，或者 api 调用失败时触发，会带上错误信息
- onPageNotFound：小程序要打开的页面不存在时触发，会带上页面信息回调该函数

8.页面相关事件处理函数

onPullDownPrefresh：监听用户下拉动作
onReachBottom：页面上拉触底事件的处理函数
onShareAppMessage：用户点击右上角转发
onPageScroll：页面滚动触发事件的处理函数

9.小程序最多只能有 5 层页面。
当我们使用 navigateTo 从父页面跳转到子页面后，就形成了 2 个页面层级。可以继续在子页面使用 navigateTO 跳转子页面。但小程序强制规定，只允许最多五层父子页面。事实上，太多的子页面将严重影响用户的产品体验。建议页面最多不要超过 3 层。
redirectTo 不存在这个问题，因为当跳转到另一个页面后，上一个页面被强制卸载掉了。

10.常见的冒泡事件：
touchstart：手动触摸动作开始
touchmove：手动触摸后移开
touchcancle：手动触摸动作被打断，如来电提醒，弹窗
touchhend：手动触摸动作结束
tap：手动触摸后马上离开
longpress：手动触摸后，超过 350ms 再离开，如果指定了事件回调函数并触发了这个事件，tap 事件将不被触发
longtap：手动触摸后，超过 350ms 再离开（推荐使用 longpress 事件代替）

11.组件是小程序页面的基本组成单元。其内置组件的类别有：
视图容器：view、scroll-view、swiper、movable-view、cover-view、cover-image；
基础内容：icon、text、rich-text、porgress；
表单组件：button、checkbox、form、input、label、picker、picker-view、radio、slider、switch、textarea；
导航：navgator、function-page-navigator
媒体组件：audio、image、video、camera、live-player、live-pusher
地图：map
画布：canvas
ps：一些原生组件使用时，要注意其限制。有 camera、canvas、input（仅在 focus 时）、live-player、live-pusher、map、textarea、video。

12.小程序从基础库版本 2.2.1 开始支持使用 npm 安装第三方包，因此也支持开发和使用第三方自定义组件包。

13.可以用 ES6 编写代码，编译时最好勾选 ES6 转 ES5 以避免一些兼容性问题。

### 发布、上线

大致就三步，具体可看图文[教程](https://www.jianshu.com/p/a77b73f329e4)：

- 上传代码
- 提交审核
- 发布版本

## 总结

小程序的技术属于前端技术的子集，是技术方向一个不错选择。尤其 18 年的小程序圈火爆，让各路开发者跃跃欲试。虽说小程序并不是技术上的突破，只是依托微信生态圈的繁荣。但对于绝大多数公司来讲，技术主要还是为业务服务的，不是吗。若干年后小程序会有怎样的生态还未可知。

即使公司没有这块业务，我们开发者可以先准备起来，充实自己的前端技术栈。
