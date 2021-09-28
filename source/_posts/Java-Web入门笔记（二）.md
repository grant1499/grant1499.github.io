---
title: Java Web入门笔记（二）
tags:
  - Java
categories:
  - Java
mathjax: false
copyright: true
abbrlink: b894fbe7
date: 2021-09-25 23:06:52
---

## 1.发布一个web网站

不会就先模仿。

<!--more-->

- 将自己写的网站，放到服务器（Tomcat)中指定的web应用的文件夹（webapps)下，就可以（本机）访问了。

网站文件布局：

```shell
--webapps ：Tomcat服务器的web目录
	-ROOT
	-kuangstudy ：网站的目录名
		- WEB-INF   网站的程序
			-classes : java程序
			-lib：web应用所依赖的jar包
			-web.xml ：网站配置文件
		- index.html 默认的首页，index.jsp,..,也是一样的
		- static 
            -css
            	-style.css
            -js
            -img
         -.....
```

## 2.什么是HTTP

(超文本传输协议）是一个简单的请求-响应协议，它通常运行在TCP之上。

- 文本：html，字符串，…
- 超文本：图片，音乐，视频，定位，地图.……
- 端口:80

Https:安全的    端口：443

发展历史：

- http1.0
    - HTTP/1.0：非持续连接，客户端可以与web服务器连接后，只能获得一个web资源，断开连接
- http2.0
    - HTTP/1.1：持续连接，客户端可以与web服务器连接后，可以获得多个web资源。

## 3.HTTP请求

- 客户端–发请求（Request）–服务器

以百度为例：

```http
// General, http 请求 和 响应 的请始行 的一个 公共部分
Request URL:https://www.baidu.com/   请求地址
Request Method:GET    get方法/post方法
Status Code:200 OK    状态码：200
Remote（远程） Address:14.215.177.39:443
// Request Headers, 请求头
Accept:text/html  
Accept-Encoding:gzip, deflate, br
Accept-Language:zh-CN,zh;q=0.9    语言
Cache-Control:max-age=0
Connection:keep-alive
```

请求行：

- 请求行中的请求方式：GET
- 请求方式：Get,Post,HEAD,DELETE,PUT,TRACT.…
    - get：请求能够携带的参数比较少，大小有限制，会在浏览器的URL地址栏显示数据内容，不安全，但高效
    - post:请求能够携带的参数没有限制，大小没有限制，不会在浏览器的URL地址栏显示数据内容，安全，但不高效。

消息头：

















Servlet还是有必要学的，对于后面框架的学习有帮助。

**JSP还是有必要了解一下，不需要花很多时间，知道即可**