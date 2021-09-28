---
title: JavaScript学习笔记（一）
tags:
  - JavaScript
categories:
  - - JavaScript
mathjax: true
copyright: true
abbrlink: 5f7ed589
date: 2021-08-28 12:03:56
---

## 1.前端三要素

HTML（结构）：超文本标记语言（Hyper Text Markup Language），决定网页的结构和内容

CSS（表现）：层叠样式表（Cascading Style Sheets），设定网页的表现样式。

JavaScript（行为）：是一种弱类型脚本语言，其源码不需经过编译，而是由浏览器解释运行，用于控制网页的行为。

<!--more-->

JS参考教程： https://blog.csdn.net/pan_h1995/article/details/105974385。

菜鸟教程，狂神视频教程。

一个ES6教程： https://www.runoob.com/w3cnote/es6-tutorial.html。

又一个ES6入门教程： https://es6.ruanyifeng.com/。

廖雪峰JavaScript教程： https://www.liaoxuefeng.com/wiki/1022910821149312。

推荐一个Web技术学习网站： https://developer.mozilla.org/zh-CN/docs/Web/Tutorials。

## 2.JS框架

- JQuery：大家熟知的JavaScript库，优点就是简化了DOM操作，缺点就是DOM操作太频繁，影响前端性能；在前端眼里使用它仅仅是为了兼容IE6，7，8；

- Angular：Google收购的前端框架，由一群Java程序员开发，其特点是将后台的MVC模式搬到了前端并增加了**模块化开发**的理念，与微软合作，采用了TypeScript语法开发；对后台程序员友好，对前端程序员不太友好；

- React：Facebook 出品，一款高性能的JS前端框架；特点是提出了新概念 【虚拟DOM】用于减少真实 DOM 操作，在内存中模拟 DOM操作，有效的提升了前端渲染效率；缺点是使用复杂，因为需要额外学习一门【JSX】语言；

- **Vue**：一款渐进式 JavaScript 框架，所谓渐进式就是逐步实现新特性的意思，如实现模块化开发、路由、状态管理等新特性。其特点是综合了 Angular（模块化）和React(虚拟 DOM) 的优点；

- **Axios**：前端通信框架；因为 Vue 的边界很明确，就是为了处理 DOM，所以并不具备通信能力，此时就需要额外使用一个通信框架与服务器交互；当然也可以直接选择使用jQuery 提供的AJAX 通信功能；

## 3.什么是JavaScript？

**一个合格的后端人员，必须精通JavaScript。** --- 狂神

ECMAScript它可以理解为JavaScript的一个标准

最新版本已经到es6版本~

但是大部分浏览器还只停留在支持es5代码上！

开发环境–线上环境，版本不一致

## 4.JS代码

1、html内部标签，可以用head或者body包裹

```html
<script>
    //....
    alert('hello world');
</script>
```

脚本可位于 HTML 的 <body> 或 <head> 部分中，或者同时存在于两个部分中。

通常的做法是把函数放入 <head> 部分中，或者放在页面底部。这样就可以把它们安置到同一处位置，不会干扰页面的内容。

2、外部引入hj.js

```js
alert("hello,world");
```

test.html

```html
 <!--外部引入
        注意：script必须成对出现，否则没有效果
    -->
    <script src="js/hj.js"></script>

    <!--不用显示定义type，也默认就是javaScript-->
    <script type="text/javascript"></script>
```

## 5.数据类型与变量

**数字（Number）字面量** 可以是整数或者是小数，或者是科学计数(e)。

如3.14，1001，1e5。还有NaN（not a number），Infinity。

**NaN与所有数值都不等，包括自己。只能用isNaN(x)函数判断x是否属于NaN。**

JS中1/3会返回浮点数，这与Java不一样，和python类似。

关于浮点数，JS也存在精度丢失问题，如`1/3 === 1 - 2/3`会返回false。

通过`console.log(Math.abs(1/3 - (1 - 2/3)) < 0.001);`就会返回true。

**字符串（String）字面量** 可以使用单引号或双引号。

**布尔（Boolean）字面量** 只能有两个值：true 或 false。

**数组（Array）字面量** 定义一个数组：

`[40, 100, 1, 5, 25, 10]`

**对象（Object）字面量** 定义一个对象：

`{firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"}`

**函数（Function）字面量** 定义一个函数：

`function myFunction(a, b) { return a * b;}`

JavaScript 使用关键字 **var** 来定义**变量**， 使用等号来为变量赋值：

在 2015 后的 JavaScript 版本 (ES6) 允许我们使用 const 关键字来定义一个常量，使用 let 关键字定义的限定范围内作用域的变量。

```js
// 方式1
var x, length;// 在计算机程序中，经常会声明无值的变量。未使用值来声明的变量，其值实际上是 undefined。
x = 5;
length = 6;
// 方式2
var x = 5;
var length = 6;
/*如果重新声明 JavaScript 变量，该变量的值不会丢失：
在以下两条语句执行后，变量 carname 的值依然是 "Volvo"：*/
var carname="Volvo";
var carname;
// 为多个变量赋同一个值
var x,y,z=1;
```

```js
var length = 16;                                  // Number 通过数字字面量赋值
var points = x * 10;                              // Number 通过表达式字面量赋值
var lastName = "Johnson";                         // String 通过字符串字面量赋值
var cars = ["Saab", "Volvo", "BMW"];              // Array  通过数组字面量赋值
var person = {firstName:"John", lastName:"Doe"};  // Object 通过对象字面量赋值
```

## 6.if-else分支语句

```js
<script>
    //  1. 定义变量   变量类型 变量名 = 变量值
    var score = 1 ;
// alert(num)
//  2. 条件控制
// 与Java类似的写法
if (score > 60 && score < 70){
    alert("60~70");
}else if(score > 70 && score < 80){
    alert("70~80");
}else{
    alert("other");
}

console.log(score);
</script>
```

**注意：JavaScript严格区分大小写！**

## 7.注释

单行：`//`，多行：`/**/`

## 8.浏览器执行JS

参考资料： https://www.runoob.com/js/js-chrome.html。

打开浏览器，F12或者右键检查。

选择Console窗口，在`>`之后输入`console.log("hello,world!")`，回车。

JavaScript 可以通过不同的方式来输出数据：

- 使用 **window.alert()** 弹出警告框。
- 使用 **document.write()** 方法将内容写到 HTML 文档中。
- 使用 **innerHTML** 写入到 HTML 元素。
- 使用 **console.log()** 写入到浏览器的控制台。

选择Sources窗口可以查看源码，可以在这个界面调试js代码，打断点等。

## 9.比较运算符

==：等于

===：**绝对等于（值和类型均相等）**

!==： 不绝对等于（值和类型有一个不相等，或两个都不相等）

其他同Java。

我们大多数情况用`===`判断是否相等，因为`1=="1"`2个等号会判断相等。

这是JS的一个缺陷，尽量不要使用`==`，用`===`替代。

## 10.null和undefined

null表示空，undefined表示未定义。