---
title: Java Web入门笔记（五）
tags:
  - Java
categories:
  - Java
mathjax: false
copyright: true
abbrlink: 2d398497
date: 2021-10-14 11:33:43
---

## 1.实现重定向（重要）

<!--more-->

重定向（Redirect），常用于用户登陆

状态码：302

```java
	// 第一种：使用示例
	response.setStatus(302);
	response.setHeader("Location", "login_success.html");*/
		
	// 第二种：使用示例
	response.sendRedirect("login_success.html"); // 注意路径设置好

	1. 地址上显示的是最后的那个资源的路径地址。
	2. 请求次数最少有两次，服务器在第一次请求后，会返回302以及一个地址，浏览器在根据这个地址，执行第二次访问。
	3. 可以跳转到任意路径，不是自己的工程也可以跳。
	4. 效率稍微低一点，执行两次请求。 
	5. 后续的请求，没法使用上一次的request存储的数据，或者没法使用上一次的request对象，因为这是两次不同的请求。
```

请求转发（Forward）

```java
	// 使用示例：
	request.getRequestDispatcher("login_success.html").forward(request, response);

	1. 地址上显示的是请求servlet的地址，返回200ok。
	2. 请求次数只有一次，因为是服务器内部帮客户端执行了后续的工作。 
	3. 只能跳转自己项目的资源路径。  
	4. 效率上稍微高一点，因为只执行一次请求。 
	5. 可以使用上一次的request对象。
```

## 2.HttpServletResponse接口

HttpServletRequest代表客户端的请求,用户通过Http协议访问服务器, HTTP请求中的所有信息会被封装到HttpServletRequest,通过这个HttpServletRequest的方法,获得客户端的所有信息。

```java
// 两个方法获取客户端传过来的参数，一个返回String，一个返回String[]
String username = req.getParameter("username");
String[] messages = req.getParameterValues();
```

### 模拟登录流程

`web.xml`中添加内容：

```xml
<servlet>
    <servlet-name>request</servlet-name>
    <servlet-class>top.grantdrew.RequestTest</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>request</servlet-name>
    <url-pattern>/login</url-pattern>
</servlet-mapping>
```

修改默认主页`index.jsp`：

```jsp
<html>
<body>
<h2>Hello World!</h2>

<%--action这里提交的路径需要寻找到项目的路径--%>
<%--${pageContext.request.contextPath}是el表达式，代表当前的项目--%>
<%@ page pageEncoding="UTF-8"%>   <%--设置UTF-8防止乱码--%>
<form action="${pageContext.request.contextPath}/login" method="get">
    <p>用户名：<input type="text" name="username"></p>
    <p>密&nbsp;&nbsp;&nbsp;码：<input type="password" name="password"></p>
    <input type="submit">
    <input type="reset">
</form>
</body>
</html>
```

在`webapp`目录下新建`success.jsp`文件作为成功登录后的页面：

```jsp
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h1>Success!</h1>
</body>
</html>
```

新建`RequestTest.java`来处理登录请求：

```java
public class RequestTest extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("进入表单提交请求...");
        // 处理登录请求
        String username = req.getParameter("username");
        String pwd = req.getParameter("password");

        System.out.println(username + ":" + pwd); // 控制台输出用户名和密码
        resp.sendRedirect("/servlet_02_war/success.jsp"); // 重定向404注意修改路径
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

### 获取参数请求转发

在`webapp`目录下修改`index.jsp`文件作为登录页面：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Login Page</title>
    <h1>登录</h1>

    <div style="text-align: center">
        <form action="${pageContext.request.contextPath}/login" method="post">
            <% 这里的/login就是下面的处理登录程序ServletDemo_03映射到的路径 %>
            <p>用户名：<input type="text" name="username"></p>
            <p>密码：<input type="password" name="pwd"></p>
            <p>爱好：
                <input type="checkbox" value="sleep" name="hobby" checked>睡觉
                <input type="checkbox" value="basketball" name="hobby">篮球
                <input type="checkbox" value="soccer" name="hobby">足球
                <input type="checkbox" value="art" name="hobby">艺术
            </p>
            <input type="submit">
            <input type="reset">
        </form>
    </div>
</head>
<body>

</body>
</html>
```

新建`ServletDemo_03.java`来处理登录请求：

```java
public class ServletDemo_03 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        String pwd = req.getParameter("pwd");
        String[] hobbies = req.getParameterValues("hobby");// 处理多选框
        System.out.println("====================");
        System.out.println(username);
        System.out.println(pwd);
        System.out.println(Arrays.toString(hobbies));
        // 将请求转发，跳转到登录成功后的页面，注意：请求转发不需要加项目路径，因为这是站内转发，重定向才要加
        req.getRequestDispatcher("/success.jsp").forward(req,resp);
    }
}
```

## 3.Cookie与Session概述

参考教程： https://www.liaoxuefeng.com/wiki/1252599548343744/1328768897515553。

因为HTTP协议是一个无状态协议，即Web应用程序无法区分收到的两个HTTP请求是否是同一个浏览器发出的。所有需要**Session来记录用户身份**。

我们把这种基于唯一ID识别用户身份的机制称为**Session**。每个用户第一次访问服务器后，会自动获得一个Session ID。如果用户在一段时间内没有访问服务器，那么Session会自动失效，下次即使带着上次分配的Session ID访问，服务器也认为这是一个新用户，会分配新的Session ID。

Session的意思也就是会话，Tmux中也有Session的概念。

而服务器识别Session的关键就是依靠一个名为`JSESSIONID`的Cookie。在Servlet中第一次调用`req.getSession()`时，Servlet容器自动创建一个Session ID，然后通过一个名为`JSESSIONID`的Cookie发送给浏览器。通过`JSESSIONID`这个Cookie识别Session是Tomcat的做法，现在逐渐被淘汰。

**Cookie其实是一份小数据，它是服务器给客户端并且存储在客户端上的一份小数据。**

> 客户端 与 服务端
>
> 1. 服务端给客户端一个信件，客户端下次访问服务端带上信件就可以了； cookie
> 2. 服务器登记你来过了，下次你来的时候我来匹配你； seesion

Cookie存在客户端，Session存在服务器。