---
title: HTML学习笔记（三）
date: 2021-05-04 22:16:11
tags: [HTML]
categories: 
	- [HTML]
mathjax: true
copyright: true
---

## 1.页面结构分析

![image-20210504222223240](HTML学习笔记（三）/image-20210504222223240.png)

<!--more-->

头部、脚部和导航比较总要。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>页面结构分析</title>
</head>
<body>
<header>
    <h2>网页头部</h2>
</header>
<section>
    <h2>网页主体</h2>
</section>
<footer>
    <h2>网页脚部</h2>
</footer>
</body>
</html>
```

## 2.iframe内联框架

![image-20210504223521277](HTML学习笔记（三）/image-20210504223521277.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>内联框架</title>
</head>
<body>
<!--内联框架
src: 地址
w-h：宽-高度
name：标识名
-->
<iframe src="https://www.baidu.com/"
        name="hello"
        frameborder="0"
        width="1200px" height="800px">
</iframe>
<a href="媒体元素.html" target="hello">点击跳转</a>
</body>
</html>
```

## 3.表单语法

![image-20210504224535465](HTML学习笔记（三）/image-20210504224535465.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>表单学习</title>
</head>
<body>
<h2>注册</h2>
<!--表单form
action: 表单提交的位置，可以是网站，也可以是一个请求处理地址
method：post，get 提交方式
get方式：可以在提交位置的url看到提交信息，不安全，高效
post方法：比较安全，传输大文件，可以在F12看到密码等信息
-->
<form action="表格.html" method="post">
<!--文本输入框：input type="text"-->
    <p>名字：<input type="text" name="username"></p>
<!--密码框：input type="password"-->
    <p>密码：<input type="password" name="pwd"></p>
    <p>
<!--提交和重置-->
        <input type="submit">
        <input type="reset">
    </p>
</form>
</body>
</html>
```

在填写注册信息时，打开F12，查看network-->Headers.

![image-20210504230152083](HTML学习笔记（三）/image-20210504230152083.png)