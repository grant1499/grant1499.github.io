---
title: CSS学习笔记（五）
tags:
  - CSS
categories:
  - - CSS
mathjax: true
copyright: true
abbrlink: 24ded0f3
date: 2021-08-26 11:38:51
---

## 1.定位

### 1.1相对定位

相对定位：positon：relative；
相对于**自己**原来的位置，进行指定的偏移，相对定位的话，它仍然在标准文档流中，原来的位置会被保留，不会发生塌陷。

<!--more-->

> 标准文档流：**文档流**指的是元素排版布局过程中，元素会**默认**自动从左往右，从上往下的**流式排列方式**。并最终窗体自上而下分成一行行，并在每行中从左至右的顺序排放元素。

```css
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        div{
            margin: 10px;
            padding: 5px;
            font-size: 12px;
            line-height: 25px;
        }
        #father{
            border: 1px solid black;
        }
        #first{
            border: 1px dashed pink;
            position: relative;/*相对定位*/
            top: -10px;/*往自己原来位置的上方填充-10px，等价于向上走10px*/
            left: 20px;/*往自己原来位置的左方填充20px，等价于向右走20px*/
        }
        #second{
            border: 1px dashed gray;
        }
        #third{
            border: 1px dashed #8726b0;
        }
    </style>

</head>
<body>

<div id="father">
    <div id="first">第一个盒子</div>
    <div id="second">第二个盒子</div>
    <div id="third">第三个盒子</div>
</div>

</body>
```

注意：top，bottom，left，right这些值表示的是到原来位置的距离。

### 1.2小练习

line-height是行高的意思，它决定了**元素中文本内容的高度**，height则是定义**元素自身的高度**。

- **height：表示 行高**
- **line-height：表示  每行文字所占的高度** 

```html
<head>

    <style>
        #box{
            width: 300px;
            height: 300px;
            border: 1px solid red;
            margin: 0 auto;
            padding: 8px;
        }
        a{
            width: 100px;
            height: 100px;/*决定链接框的高度，需要display才能展示出来*/
            color: white;
            background-color: #d0a0f5;
            text-decoration: none;
            line-height: 100px;/*文字所占高度*/
            text-align: center;/*文字居中*/
            display: block;/*行内元素展开成块元素*/
        }
        a:hover{
            background-color: #4f4fed;
        }
        .second,.forth{
            position: relative;
            left: 200px;
            top: -100px;
        }
        .fifth{
            position: relative;
            left: 100px;
            top: -300px;
        }
    </style>

</head>
<body>

<div id="box">
    <a class="first" href="#">链接1</a>
    <a class="second" href="#">链接2</a>
    <a class="third" href="#">链接3</a>
    <a class="forth" href="#">链接4</a>
    <a class="fifth" href="#">链接5</a>
</div>

</body>
```

效果图：

![image-20210828110506165](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232035501.png)

### 1.3绝对定位

绝对定位：position: absolute;

定位：基于xxx定位，上下左右~
1、没有父级元素定位的前提下，相对于浏览器定位
2、假设父级元素存在定位，我们通常会相对于父级元素进行偏移
3、在父级元素范围内移动，最好不要超出

总结：相对一父级或浏览器的位置，进行指定的偏移，绝对定位的话，它不在标准文档流中，原来的位置不会被保留。

通常会在父级元素加上`position: relative;`不会对父级造成影响，又能实现子元素相对父级绝对定位。

```css
#father{
            border: 1px solid #666;
            padding: 0;
            position: relative;
 }
#second{
            background-color: green;
            border: 1px dashed #0ece4f;
            position: absolute;
            right:30px;
            top:30px
}
```

### 1.4固定定位

固定定位：position: fixed;

```css
<style>
div:nth-of-type(1){
    width: 100px;
    height: 100px;
    position: absolute;/*绝对定位：没有相对的父级元素，所以相对于浏览器，有滚动条会动*/
    right: 0;
    bottom: 0;
    background-color: #f39cff;
}
div:nth-of-type(2){
    width: 50px;
    height: 50px;
    position: fixed;/*固定定位：有滚动条也不动*/
    right: 0;
    bottom: 0;
    background-color: #3676a7;
}
</style>
<body>

<div>dvi1</div>
<div>dvi2</div>

</body>
```

### 1.5 z-index

![image-20210828112649561](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232035541.png)

图层~

z-index：默认是0，最高无限~999

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="css/style.css">

</head>
<body>
<div id="content">
    <ul>
        <li><img src="images/bg.jpg" alt=""></li>
        <li class="tipText">学习微服务，找狂神</li>
        <li class="tipBg"></li>
        <li>时间：2099-01=01</li>
        <li>地点：月球一号基地</li>
    </ul>
</div>
</body>
</html>
```

```css
#content{
    width: 380;
    padding: 0px;
    margin: 0px;
    overflow: hidden;
    font-size: 12px;
    line-height: 25px;
    border: 1px solid yellow;
}
ul,li{
    padding: 0px;
    margin: 0px;
    list-style: none;
}
/*父级元素相对定位*/
#content ul{
    position: relative;
}
.tipText,.tipBg{
    position: absolute;
    width: 380px;
    height: 25px;
    top:216px
}
.tipText{
    color: white;
    z-index: 999;/*使得文字浮在背景图层上方*/
}
.tipBg{
    background: orange;
    opacity: 0.5;/*背景透明度*/
    filter: alpha(opacity=50);
}
```

## 2.CSS总结

![img](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232035563.png)

CSS简单入门系列到这里正式结束！！！