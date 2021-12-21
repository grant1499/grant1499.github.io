---
title: Java Web入门笔记（四）
tags:
  - Java
categories:
  - Java
mathjax: false
copyright: true
abbrlink: 42a741d0
date: 2021-10-04 19:45:18
---

## 1.再新建一个Servlet模块

<!--more-->

在工程下新建一个同级Maven web模块。

配置参考前一篇笔记，和第一个Maven web模块类似。

配置Tomcat时，每次只配置一个模块的war包，节省加载时间。

![image-20211013162713701](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232057330.png)

## 2.ServletContext接口

参考博客： https://blog.csdn.net/qq_36371449/article/details/80314024。

web容器在启动的时候，它会为每个web程序都创建一个对应的ServletContext对象，它代表了当前的web应用。

服务器会为**每一个工程创建一个对象**，这个对象就是ServletContext对象。这个对象**全局唯一**，而且工程内部的所有servlet都共享这个对象。所以叫**全局应用程序共享对象**。

当从服务器移除托管或者是关闭服务器时，ServletContext将会被销毁。它主要有以下几方面作用：

1. 获取全局配置参数
2. 获取web工程中的资源
3. 在servlet间共享数据域对象，这个Servlet中保存的数据，可以在另外一个servlet中拿到

### 共享数据域对象

如果没有ServletContext，可以借助文件IO从一个Servlet读取另一个Servlet的数据；有了ServletContext就可以直接共享。

```java
public class ServletTest extends HttpServlet {
    // 重写doGet和doPost方法
    // 由于get或者post只是请求实现的不同的方式，可以相互调用，业务逻辑都一样
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();

        String username = "秦疆";
        context.setAttribute("username",username);//将一个数据保存在了ServletContext
        // 名字为：username ; 值 username
    }
}
```

```java
public class GetServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();
        String username = (String) context.getAttribute("username");// 拿到数据
        resp.setContentType("text/html");
        resp.setCharacterEncoding("utf-8");
        resp.getWriter().print("<h1>名字："+username + "</h1>");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

`web.xml`片段：

```xml
<servlet>
    <servlet-name>hello</servlet-name>
    <servlet-class>top.grantdrew.ServletTest</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>hello</servlet-name>
    <url-pattern>/hello</url-pattern>
</servlet-mapping>

<servlet>
    <servlet-name>getc</servlet-name>
    <servlet-class>top.grantdrew.GetServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>getc</servlet-name>
    <url-pattern>/getc</url-pattern>
</servlet-mapping>
```

先访问`/hello`，再访问`/getc`。

测试结果：

![image-20211013171829556](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232058044.png)

### 获取初始化参数

第一步：在web.xml中新增以下代码

```xml
<!--配置一些web应用初始化参数-->
<context-param>
    <param-name>url</param-name>
    <param-value>jdbc:mysql://localhost:3306/mybatis</param-value>
</context-param>
```

第二步：在doGet方法中新增以下代码

```java
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    ServletContext context = this.getServletContext();

    String url = context.getInitParameter("url");
    resp.getWriter().print(url);
}
```

### 请求转发

```java
public class ServletDemo04 extends HttpServlet {
 	@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();
        System.out.println("进入了ServletDemo04");
        //RequestDispatcher requestDispatcher = context.getRequestDispatcher("/gp"); 		 //转发的请求路径
        //requestDispatcher.forward(req,resp); //调用forward实现请求转发；
        context.getRequestDispatcher("/gp").forward(req,resp);
    }   // 访问/gp会得到/ServletDemo04一样的内容
}
```

注意转发和重定向的区别：

转发不改变浏览器地址栏，重定向改变浏览器地址栏。

![image-20211013182446968](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232058631.png)

### 读取Web工程中的资源文件Properties

- 在src/main/resources目录下新建db.properties
- 清空target后，重新生成，db.properties在WEB-INF/classes路径下
- 可以发现java目录和resources目录下的内容都被打包到了同一个路径下：classes，我们俗称这个路径为classpath

![image-20211013185019066](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232058296.png)

---

1.如何清空target打包目录？

![image-20211013193947257](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232058863.png)

2.当你把db.properties放在java目录下时，由于Maven的标准结构**约定大于配置**，可能出现资源导出失败的问题：

在`<build></build>`中配置resources，来防止资源导出失败的问题。

```xml
<build>  <!-- 把<build></build>代码删掉，复制这段-->
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

---

```java
public class ServletDemo extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        InputStream is = this.getServletContext().getResourceAsStream("/WEB-INF/classes/db.properties"); // 第一个 “/” 表示打包后的项目根目录
        Properties prop = new Properties();
        prop.load(is); // 通过文件流读取打包好后的配置文件

        String user = prop.getProperty("username");
        String pwd = prop.getProperty("password");
        resp.getWriter().print("user:" + user + ",pwd:" + pwd);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

测试一下，输出成功！

## 3.HttpServletResponse接口

HttpServletResponse这个对象负责返回数据给客户端。

查看类的源码：

前面定义的一些常量是HTTP响应状态码

负责向浏览器发送数据的方法

```java
servletOutputstream getOutputstream() throws IOException;
Printwriter getwriter() throws IOException;
```

负责向浏览器发送响应头的方法

```java
void setCharacterEncoding(String var1)；
void setContentLength(int var1)；
void setContentLengthLong(long var1);
void setContentType(String var1)；
void setDateHeader(String varl,long var2)
void addDateHeader(String var1,long var2)
void setHeader(String var1,String var2);
void addHeader(String var1,String var2)；
void setIntHeader(String var1,int var2);
void addIntHeader(String varl,int var2);
```

**Servlet下载文件测试**：忘了的可以简单回顾一下Java IO流。

弹出文件下载框，成功！将文件名改成中文也测试成功！

```java
public class FileServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.要获取下载文件的路径
        String realPath = this.getServletContext().getRealPath("/WEB-INF/classes/1.jpg"); // 第一个“/” 表示打包后的项目根目录
        //2.下载的文件名是啥？
        String filename = realPath.substring(realPath.lastIndexOf("\\") + 1);// lastIndexOf获取最后一次出现的索引
        //3.设置想办法让浏览器能够支持下载我们需要的东西，响应首部字段，中文文件名要处理乱码
        resp.setHeader("Content-Disposition","attachment;filename="+ URLEncoder.encode(filename,"UTF-8"));
        //4.获取下载文件的输入流
        FileInputStream fis = new FileInputStream(realPath);
        //5.创建缓冲区
        int len = 0;
        byte[] buffer = new byte[1024];
        //6.获取OutputStream对象
        ServletOutputStream sout = resp.getOutputStream();
        //7.将FileOutputStream流写入到buffer缓冲区
        //8.使用OutputStream将缓冲区中的数据输出到客户端
        while ((len = fis.read(buffer)) > 0){
            sout.write(buffer,0,len);
        }
        sout.close();
        fis.close();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

查看HTTP响应报文的首部字段：

![image-20211014101910978](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232058931.png)

**实现验证码功能：**

- 前端实现，用JS判断
- 后端实现，需要用到Java的图片类，生产一个图片

```java
public class ServletDemo_02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 如何让浏览器3秒刷新一次
        resp.setHeader("refresh","3");
        // 图片类，在内存中创建一个图片，属于GUI内容
        BufferedImage image = new BufferedImage(80,20,BufferedImage.TYPE_INT_RGB);
        // 得到图片，开始画图
        Graphics2D g = (Graphics2D) image.getGraphics();
        // 设置图片背景颜色
        g.setColor(Color.white); // 画笔设为白色填充背景
        g.fillRect(0,0,80,20);
        // 给图片写数据
        g.setColor(Color.blue);
        g.setFont(new Font(null,Font.BOLD,16));
        g.drawString(makeNumber(),0,20);
        // 告诉浏览器请求以图片形式打开
        resp.setContentType("image/jpg");
        // 网站存在缓存，设置浏览器禁止缓存，固定写法，抄就完了
        resp.setDateHeader("expires",-1);
        resp.setHeader("Cache-Control","no-cache");
        resp.setHeader("Pragma","no-cache");
        //把图片写给浏览器
        ImageIO.write(image,"jpg", resp.getOutputStream());
    }
    // 生成随机数验证码
    private String makeNumber(){
        Random random = new Random();
        // 生成0~9999999的随机数
        String number = random.nextInt(10000000) + "";
        StringBuffer sb = new StringBuffer();
        for (int i = 0;i < 7 - number.length();i ++){
            sb.append(0); // 生成7位数的验证码
        }
        return sb.toString() + number;
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

测试：浏览器3秒刷新验证码。

![image-20211014113011678](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232059784.png)