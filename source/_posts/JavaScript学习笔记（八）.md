---
title: JavaScript学习笔记（八）
tags:
  - JavaScript
categories:
  - - JavaScript
mathjax: true
copyright: true
abbrlink: baa05224
date: 2021-09-06 22:00:40
---

## 1.什么是jQuery

JavaScript世界中使用最广泛的一个库。

jQuery的理念“Write Less, Do More“，让你写更少的代码，完成更多的工作！

<!--more-->

在线CDN引入jQuery： https://www.jq22.com/cdn/#contact。

百度CDN：`<script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>`。

jQuery可以在线引入，也可以本地使用（需要下载）。

jQuery库，里面存在大量的JavaScript函数。

## 2.使用jQuery

在`head`中引入jQuery：

```html
<head>
    <script src="//code.jquery.com/jquery-1.11.3.min.js"></script>
	...
</head>
```

`$`是著名的jQuery符号。实际上，jQuery把所有功能全部封装在一个全局变量`jQuery`中，而`$`也是一个合法的变量名，它是变量`jQuery`的别名。`$`本质上就是一个函数，但是函数也是对象，于是`$`除了可以直接调用外，也可以有很多其他属性。

jQuery的公式：**公式：`$(selector).action()`**。选择器+事件。

## 3.jQuery选择器

选择器是jQuery的核心。一个选择器写出来类似`$('#dom-id')`。

DOM操作很繁琐，找到一个DOM节点可能需要层层定位。

jQuery的选择器就是帮助我们快速定位到一个或多个DOM节点。

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
</head>
<body>
    <a href="" id="test-jquery">点我</a>
    <script>
        // 选择器就是CSS中的选择器
        // 按id查找，返回jQuery对象
        // 如果id不存在，返回[]
        $('#test-jquery').click(function () {
            alert('hello,jQuery!');
        });
    </script>
</body>
</html>
```

对应CSS中的选择器：

```js
//原生js，选择器少，麻烦不好记
//标签
document.getElementByTagName();
//id
document.getElementById();
//class
document.getElementByClassName();

//jQuery css中的选择器它全部都能用！
$('p').click();//标签选择器
$('#id1').click();//id选择器
$('.class1').click;//class选择器
```

https://jquery.cuishifeng.cn/index.html。（jQuery工具站，哪里不会点哪里）

## 4.jQuery事件

因为JavaScript在浏览器中以单线程模式运行，页面加载后，一旦页面上所有的JavaScript代码被执行完后，就只能依赖触发事件来执行JavaScript代码。

浏览器在接收到用户的鼠标或键盘输入后，会自动在对应的DOM节点上触发相应的事件。如果该节点已经绑定了对应的JavaScript处理函数，该函数就会自动调用。

由于不同的浏览器绑定事件的代码都不太一样，所以用jQuery来写代码，就屏蔽了不同浏览器的差异，我们总是编写相同的代码。

jQuery事件主要分为：鼠标事件，键盘事件和其他事件。

```js
mousedown()(jQuery)----按下
mouseenter()(jQuery)---进入
mouseleave()(jQuery)---移出
mousemove()(jQuery)----移动
click: 鼠标单击时触发
dblclick：鼠标双击时触发
mouseout()(jQuery)
mouseover()(jQuery)
mouseup()(jQuery)
```

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
    <style>
        #divMove{
            width: 500px;
            height: 500px;
            border: 1px solid red;
        }
    </style>
</head>
<body>
    <!-- 获取鼠标当前的坐标 -->
    mouse: <span id="mouseMove"></span>
    <div id="divMove">
        在这里移动鼠标试试
    </div>
    
    <script>
        // 当网页元素加载完毕之后，响应事件
        // $(document).ready(function(){})，可以简化成下面的形式
        $(function(){
            $('#divMove').mousemove(function(e){
                $('#mouseMove').text('x:'+e.pageX + ',y:'+e.pageY);
            })
        })
    </script>

</body>
</html>
```

![image-20210920114950081](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232126934.png)

## 5.jQuery操作DOM元素

原生JS操作DOM元素比较麻烦。

DOM节点的文本操作：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
    
</head>
<body>
    <ul id="test-ul">
        <li class="js">JavaScript</li>
        <li name="python">Python</li>
        <li id="java">Java</li>
        <li>C#</li>
    </ul>
    <script>
        // 层次选择器，两个DOM元素具有层级关系，用空格隔开，选择特定属性用[]
        $('#test-ul li.js').text(123);
        $('#test-ul li[name=python]').text(456);
    </script>

</body>
</html>
```

```js
$('#test-ul li[name=python]').text();//获得值
$('#test-ul li[name=python]').text('设置值');//设置值
$('#test-ul').html();//获得值
$('#test-ul').html('<strong>123</strong>');//设置值
```

DOM节点的CSS样式操作：

```js
$('#test-ul li[name=python]').css({"color","red"});
```

元素的显示和隐藏：本质style中的`display:none`

```js
$('#test-ul li[name=python]').show();
$('#test-ul li[name=python]').hide();
```

## 6.前端三剑客总结

1、如何巩固JS（看jQuery源码，看游戏源码！）

2、巩固HTML、CSS（扒网站，全部down下来，然后对应修改看效果~）

扒网页技巧，看源码，删掉多余语句，看看是否影响框架。

Layer弹窗组件。

Element组件。

前端入门结束，完结撒花~~