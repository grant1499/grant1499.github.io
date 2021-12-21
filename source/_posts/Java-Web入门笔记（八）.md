---
title: Java Web入门笔记（八）
tags:
  - Java
categories:
  - Java
mathjax: false
copyright: true
abbrlink: 6d5c9092
date: 2021-10-15 16:33:42
---

## 1.EL表达式

EL是为了简化咱们的jsp代码，具体一点就是为了简化在jsp里面写的那些java代码。

<!--more-->

注意：如果从作用域中取值，会先从小的作用域开始取，如果没有，就往下一个作用域取，一直把四个作用域取完都没有，就没有显示。

语法：${ 表达式 }

EL表达式的11个内置对象：

| **隐含对象**     | **描述**                   |
| ---------------- | -------------------------- |
| pageScope        | page 作用域                |
| requestScope     | request 作用域             |
| sessionScope     | session 作用域             |
| applicationScope | application 作用域         |
| param            | Request 对象的参数，字符串 |
| paramValues  | Request 对象的参数，字符串集合 |
| header       | HTTP 信息头，字符串            |
| headerValues | HTTP 信息头，字符串集合        |
| initParam    | 上下文初始化参数               |
|cookie	            |      Cookie值       |
|pageContext	|当前页面的pageContext|

除了最后一个都是Map类型。

**如果域中所存的是对象：**

```jsp
<%
    User u = new User();
    u.setUsername("zhangsan");
    u.setPassword("123456");

    pageContext.setAttribute("u", u);
    request.setAttribute("u", u);
    session.setAttribute("u", u);
    application.setAttribute("u", u);
%>

<br>使用普通手段取出作用域中的值<br>

<%= ((User) pageContext.getAttribute("u")).getUsername() %>
<%= ((User) request.getAttribute("u")).getUsername() %>
<%= ((User) session.getAttribute("u")).getUsername() %>
<%= ((User) application.getAttribute("u")).getUsername() %>

<br>使用EL表达式取出作用域中的值<br>

<p>${ pageScope.u.username }</p>
<p>${ requestScope.u.username }</p>
<p>${ sessionScope.u.username }</p>
<p>${ applicationScope.u.username }</p>
```

**如果域中所存的是键值：**

```jsp
<%
    pageContext.setAttribute("name", "page");
    request.setAttribute("name", "request");
    session.setAttribute("name", "session");
    application.setAttribute("name", "application");
%>

<br>使用普通手段取出作用域中的值<br>

<%= pageContext.getAttribute("name") %>
<%= request.getAttribute("name") %>
<%= session.getAttribute("name") %>
<%= application.getAttribute("name") %>

<br>使用EL表达式取出作用域中的值<br>

${ pageScope.name }
${ requestScope.name }
${ sessionScope.name }
${ applicationScope.name }	
```

## 2.JSTL表达式

JSTL（JSP Standard Tag Library，标准标签库）主要也是为了简化jsp的代码编写，替换 <%%> 写法，一般与EL表达式配合使用。功能等效于Java代码。

1. 导入依赖库：jstl.jar、standard.jar
2. 引入标签库：`<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`

常用标签：

- c:set

```jsp
<!-- 声明一个对象 myname，对象的值 zhangsan，存储到了 page（默认），指定是 session 域 -->
<c:set var="myname" value="zhangsan" scope="session"></c:set>
${ sessionScope.myname }
```

- c:if

```jsp
<c:set var="age" value="18" ></c:set>
<c:if test="${ age > 26 }">
    年龄大于了26岁...
</c:if>
<c:if test="${ age <= 26 }">
    年龄小于了26岁...
</c:if>

<%--定义一个变量名 flag，去接收前面表达式的值,然后存在 session 域中--%>
<c:if test="${ age > 26 }" var="flag" scope="session">
    年龄大于了26岁...
</c:if>
```

- c:forEach

```jsp
<%--从1开始遍历到10，得到的结果，赋值给 i，并且会存储到page域中，step代表增幅为2--%>
<c:forEach begin="1" end="10" var="i" step="2">
    ${i}
</c:forEach>

<%
    List<User> list = new ArrayList<User>();

    User u1 = new User();
    u1.setUsername("zhangsan");
    u1.setPassword("123456");
    list.add(u1);

    User u2 = new User();
    u2.setUsername("lisi");
    u2.setPassword("123456");
    list.add(u2);

    request.setAttribute("list",list);
%>
<!--items：表示遍历哪一个对象，注意这里必须写EL表达式。
	var：遍历出来的每一个元素用user去接收。 -->
<c:forEach var="user" items="${ list }">
    ${ user.username } ---- ${ user.password } <br />
</c:forEach>
```

## 3.JSP JavaBean

JavaBean 是特殊的 Java 类。

作用：主要用来传递数据，即把一组数据组合成一个JavaBean便于传输。

JavaBean有特定的写法：

- 必须要有一个无参构造
- 属性必须私有化
- 必须有对应的get/set方法

一般用来和数据库的字段做映射 ORM。

ORM ：对象关系映射

- 表—>类
- 字段–>属性
- 行记录---->对象

新建数据库：

![image-20211017195605637](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232049690.png)

新建表：

![image-20211017200023761](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232049728.png)

创建JavaBean项目，新建`People`类：这就是一个JavaBean。

```java
package top.grantdrew;

public class People {
    private int id;
    private String name;
    private int age;
    private String address;

    public People(int id, String name, int age, String address) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.address = address;
    }

    public People(){

    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "People{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                ", address='" + address + '\'' +
                '}';
    }
}
```

创建`javabean,jsp`测试：

```jsp
<body>

<%--
 People people = new People(); 等价于下面的
 people.setAddress();
 people.setId();
 people.setAge();
 people.setName();
--%>
<jsp:useBean id="people" class="top.grantdrew.People" scope="page"/>

<jsp:setProperty name="people" property="address" value="南京"/>
<jsp:setProperty name="people" property="id" value="001"/>
<jsp:setProperty name="people" property="age" value="19"/>
<jsp:setProperty name="people" property="name" value="Tom"/>

<p>姓名：<jsp:getProperty name="people" property="name"/></p>
<p>年龄：<jsp:getProperty name="people" property="age"/></p>
<p>编号：<jsp:getProperty name="people" property="id"/></p>
<p>地址：<jsp:getProperty name="people" property="address"/></p>

</body>
```

## 4.MVC三层架构

MVC教程： https://www.liaoxuefeng.com/wiki/1252599548343744/1266264917931808。

我们通过前面的章节可以看到：

- Servlet适合编写Java代码，实现各种复杂的业务逻辑，但不适合输出复杂的HTML；
- JSP适合编写HTML，并在其中插入动态内容，但不适合编写复杂的Java代码。

能否将两者结合起来，发挥各自的优点，避免各自的缺点？

答案是肯定的。

什么是MVC： Model view Controller 模型、视图、控制器

![image-20211017211937055](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232049753.png)

Model

- 业务处理 ：业务逻辑（Service）
- 数据持久层：CRUD （Dao - 数据持久化对象）

View

- 展示数据
- 提供链接发起Servlet请求 （a，form，img…）

Controller （Servlet）

- 接收用户的请求 ：（req：请求参数、Session信息….）
- 交给业务层处理对应的代码
- 控制视图的跳转

```java
登录--->接收用户的登录请求--->处理用户的请求（获取用户登录的参数，username，password）---->交给业务层处理登录业务（判断用户名密码是否正确：事务）--->Dao层查询用户名和密码是否正确-->数据库
```

