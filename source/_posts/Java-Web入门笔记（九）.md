---
title: Java Web入门笔记（九）
tags:
  - Java
categories:
  - Java
mathjax: false
copyright: true
abbrlink: 1791b1cd
date: 2021-10-17 21:28:33
---

## 1.Filter

Filter是过滤器，就是对客户端发出来的请求进行过滤。浏览器发出请求，然后服务器派servlet处理。在中间就可以过滤，其实过滤器起到的是拦截的作用。使用过滤器可以对一些敏感词汇进行过滤、统一设置编码、实现自动登录等功能。

<!--more-->

![image-20211017213621794](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232051923.png)

参考博客： https://www.liaoxuefeng.com/wiki/1252599548343744/1266264823560128。

新建Maven空项目。

![image-20211017214138293](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232051961.png)

Filter开发步骤：

1. 导包
2. 编写过滤器，和servlet类似

这里导mysql，servlet，jsp，jstl和standard五个包。

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>
```

在java目录下新建两个包(servlets和filters)：

创建`CharacterEncodingFilter.java`和`ShowServlet.java`。

```java
public class CharacterEncodingFilter implements Filter {

    //初始化：web服务器启动，就已经初始化了，随时等待过滤对象出现！
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("CharacterEncodingFilter初始化！");
    }

    //Chain : 链
          /*
          1. 过滤中的所有代码，在过滤特定请求的时候都会执行
          2. 必须要让过滤器继续同行
              chain.doFilter(request,response);
           */
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        servletRequest.setCharacterEncoding("utf-8");// 处理中文编码
        servletResponse.setCharacterEncoding("utf-8");
        servletResponse.setContentType("text/html;charset=utf-8");

        System.out.println("CharacterEncodingFilter执行前！");
        filterChain.doFilter(servletRequest,servletResponse);// 放行，让请求到达下一个目标
        System.out.println("CharacterEncodingFilter执行后！");
    }
    //销毁：web服务器关闭的时候，过滤器会销毁
    @Override
    public void destroy() {
        System.out.println("CharacterEncodingFilter销毁！");
    }
}
```

```java
public class ShowServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 通过过滤器实现中文编码
        resp.getWriter().write("你好，世界！");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}
```

在web.xml中配置 Filter：

```xml
<servlet>
    <servlet-name>showservlet</servlet-name>
    <servlet-class>top.grantdrew.servlets.ShowServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>showservlet</servlet-name>
    <url-pattern>/servlet/show</url-pattern>
</servlet-mapping>

<filter>
    <filter-name>CharacterEncoding</filter-name>
    <filter-class>top.grantdrew.filters.CharacterEncodingFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>CharacterEncoding</filter-name>
    <!--只要是/servlet的任何请求，都会经过这个过滤器-->
    <url-pattern>/servlet/*</url-pattern>
</filter-mapping>
```

Filter执行顺序：

1. 客户端发出请求，先经过过滤器，如果过滤器放行，那么才能到servlet。
2. 如果有多个过滤器，那么他们会按照注册的映射顺序来排队。只要有一个过滤器不放行，那么后面排队的过滤器以及咱们的servlet都不会收到请求。如果全部放行了，那么回来的时候将会是反向执行。

**过滤器常见应用：**

用户登录之后才能进入主页！用户注销后就不能进入主页了！

1. 用户登录之后，向Sesison中放入用户的数据
2. 进入主页的时候要判断用户是否已经登录；要求：在过滤器中实现！

首先创建`login.jsp`，用户登录页面。

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>登录页面</title>
</head>
<body>
<form action="${pageContext.request.contextPath}/servlet/login" method="post">
    <p>
        用户名：<input type="text" name="username" />
    </p>
    <p>
        密码：<input type="password" name="pwd" />
    </p>
    <p>
        <input type="submit" value="提交">
    </p>
</form>
</body>
</html>
```

表单提交到`/servlet/login`由`LoginServlet.java`来处理登录请求。

```java
public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        if ("admin".equals(username)){
            // Constant.USER_SESSION用类的静态字段方便统一修改
       req.getSession().setAttribute(Constant.USER_SESSION,req.getSession().getId());
            resp.sendRedirect("/Filter_01_war_exploded/sys/success.jsp");
        }else{
            resp.sendRedirect("/Filter_01_war_exploded/error.jsp");
        }
    }
}
```

```java
// 处理登录用户，Constant.USER_SESSION用类的静态字段方便统一修改
public class Constant {
    public final static String USER_SESSION = "LOGIN_SESSION";
}
```

登录成功页面，`sys/success.jsp`：登录用户的相关页面都放在`sys`目录下由过滤器过滤。

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>个人主页</title>
</head>
<body>
    <h1>登录成功！！！</h1>
    <p><a href="${pageContext.request.contextPath}/servlet/logout">点击注销登录</a></p>
</body>
</html>
```

`sys`路径下的过滤器：

```java
public class SysFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }
    // 处理对于未登录用户对success页面的非法访问
    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) resp;

        if (request.getSession().getAttribute(Constant.USER_SESSION) == null){
            response.sendRedirect("/Filter_01_war_exploded/login.jsp");
        }
        chain.doFilter(req,resp); // 登录的用户才能放行进入主页
    }

    @Override
    public void destroy() {

    }
}
```

注销处理程序`LogoutServlet`：

```java
package top.grantdrew.filters;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class LogoutServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Object obj = req.getSession().getAttribute("LOGIN_SESSION");
        if (obj != null){
            req.getSession().removeAttribute(Constant.USER_SESSION);
        }
        resp.sendRedirect("/Filter_01_war_exploded/login.jsp");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

登录错误页面`error.jsp`：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>错误主页</title>
</head>
<body>
<h1>登录失败！</h1>
<a href="login.jsp">返回登录页面</a>
</body>
</html>
```

路径配置文件`web.xml`：

```xml
<filter>
    <filter-name>sysfilter</filter-name>
    <filter-class>top.grantdrew.filters.SysFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>sysfilter</filter-name>
    <!--只要是/servlet的任何请求，都会经过这个过滤器-->
    <url-pattern>/sys/*</url-pattern>
</filter-mapping>

<servlet>
    <servlet-name>login</servlet-name>
    <servlet-class>top.grantdrew.filters.LoginServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>login</servlet-name>
    <url-pattern>/servlet/login</url-pattern>
</servlet-mapping>

<servlet>
    <servlet-name>logout</servlet-name>
    <servlet-class>top.grantdrew.filters.LogoutServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>logout</servlet-name>
    <url-pattern>/servlet/logout</url-pattern>
</servlet-mapping>
```

## 2.Listener

Listener是监听器，监听Servlet某一个事件的发生或者状态的改变，它的内部其实就是接口回调。

Listener顾名思义就是监听器，有好几种Listener，其中最常用的是`ServletContextListener`。

监听器在GUI中经常用到，在Java Web中应用较少。

可以实现的监听器接口有非常多种，这里只举一个例子。

在java目录下新建包listens：

创建`OnlineCountListener.java`：

```java
// 统计网站在线人数，也就是统计session
public class OnlineCountListener implements HttpSessionListener {
    //创建session监听： 看你的一举一动
    //一旦创建Session就会触发一次这个事件！
    @Override
    public void sessionCreated(HttpSessionEvent se) {
        ServletContext servletContext = se.getSession().getServletContext();
        Integer onlineCount = (Integer) servletContext.getAttribute("OnlineCount");
        if (onlineCount == null){ // 注意：这里存在线程安全问题没有处理
            onlineCount = 1;
        }else{
            onlineCount ++;
        }
        servletContext.setAttribute("OnlineCount",onlineCount);
    }
    //销毁session监听
    //一旦销毁Session就会触发一次这个事件！
    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        ServletContext servletContext = se.getSession().getServletContext();
        Integer onlineCount = (Integer) servletContext.getAttribute("OnlineCount");
        if (onlineCount == null){
            onlineCount = 0;
        }else{
            onlineCount --;
        }
        servletContext.setAttribute("OnlineCount",onlineCount);
    }
}
```

编写`index.jsp`：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>Title</title>
  </head>
  <body>
    <p> 当前有 <span><%=application.getAttribute("OnlineCount") %> </span> 人在线！</p>
  </body>
</html>
```

在`web.xml`中注册监听器：

```xml
<!-- 注册监听器-->
    <listener>
        <listener-class>top.grantdrew.listeners.OnlineCountListener</listener-class>
    </listener>
```

测试一下：

如果出现在线人数不是1个人，重新部署一下Tomcat就行了。（也可能是有多个浏览器）

## 3.JDBC复习

参考MySQL学习笔记（九）。

## 4.Java Web项目（JDBC）

参考项目笔记。









还得看jquery，Json，ajax，java并发。

