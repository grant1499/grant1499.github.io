---
title: Java网络编程笔记（一）
tags:
  - Java入门
  - 网络编程
categories:
  - - Java
mathjax: true
copyright: true
abbrlink: 7285a54
date: 2021-07-30 08:41:14
---

## 1.Java网络编程

本系列主要简单概述Java网络编程，为Java web阶段打好基础。

<!--more-->

![image-20210731213349104](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232125897.png)

**实现网络通信需要解决的两个问题**

1. 如何准确地定位网络上一台或多台主机；定位主机上的特定的应用
2. 找到主机后如何可靠高效地进行数据传输

## 2.网络通信要素

**网络通信的两个要素**

对应问题一：通信双方地址，IP和端口号

对应问题二：提供网络通信协议：TCP/IP参考模型（应用层、传输层、网络层、物理+数据链路层）

网络通信协议有2套参考模型，其中OSI参考模型过于理想化，未能在因特网上进行广泛推广，TCP/IP参考模型是事实上的国际标准。

![image-20210731214817680](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232125364.png)

### 通信要素一：IP和端口号

IP的理解：

IP**:唯一的标识** Internet 上的计算机（通信实体）

在Java中使用InetAddress类代表IP

IP分类：IPv4 和 IPv6 ; 万维网 和 局域网

域名: www.baidu.com www.mi.com www.sina.com www.jd.com

本地回路地址：127.0.0.1 对应着：localhost

**InetAddress类**:

此类的一个对象就代表着一个具体的IP地址。

实例化，没有提供公共的构造器，而是提供了如下几个静态方法来获取 InetAddress实例。

```Java
getByName(String host) 、 getLocalHost()
public static InetAddress getLocalHost()// 无参数，获取本机IP
public static InetAddress getByName(String host)
```

```Java
@Test
public void test1(){
    try {// 类似File file = new File("hello.txt");
        // 方式1：IP地址
        InetAddress inet1 = InetAddress.getByName("192.168.10.14");
        System.out.println(inet1);
        // 方式2：域名，本地主机
        InetAddress inet2 = InetAddress.getByName("localhost");// www.baidu.com
        System.out.println(inet2);
    } catch (UnknownHostException e) {
        e.printStackTrace();
    }
}
```

常用方法，2个。

```Java
getHostName() / getHostAddress()
public String getHostAddress()：返回 IP 地址字符串（以文本表现形式）。
public String getHostName()：获取此 IP 地址的主机名
// -------
InetAddress inet1 = InetAddress.getByName("www.baidu.com");
System.out.println(inet1.getHostName());
System.out.println(inet1.getHostAddress());
/*www.baidu.com
36.152.44.95*/
```

端口号的理解：

正在计算机上运行的进程。

- 要求：不同的进程不同的端口号
- 范围：被规定为一个 16 位的整数 0~65535。
- 端口号与IP地址的组合得出一个网络套接字：Socket

网络编程也被称为Socket编程。

### 通信要素二：网络通信协议

![image-20210731223111621](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232125206.png)

传输层协议中有两个非常重要的协议：

传输控制协议 TCP(Transmission Control Protocol)

用户数据报协议 UDP(User Datagram Protocol) 

这两个协议都是传输层的协议，但有所不同。

![image-20210801125424296](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232125344.png)

TCP的三次握手和四次挥手

![image-20210801125823383](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232125780.png)

![image-20210801125836160](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232125375.png)

TCP类似生活中打电话，UDP类似生活中发短信。

### TCP网络编程

例子1：客户端发送信息给服务端，服务端将数据显示在控制台上。

```Java
// 实现TCP的网络编程
// 例子1：客户端发送信息给服务端，服务端将数据显示在控制台上
// 客户端
@Test
public void client() {
    Socket socket = null;
    OutputStream os = null;
    try {
        // 1.创建Socket对象，指明服务器端的ip和端口号
        InetAddress inet = InetAddress.getByName("127.0.0.1");
        socket = new Socket(inet,8899);
        // 2.获取一个输出流，用于输出数据
        os = socket.getOutputStream();
        // 3.写出数据的操作
        os.write("你好，我是客户端!".getBytes());
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        // 4.资源的关闭
        try {
            if (os != null)
                os.close();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                if (socket != null)
                    socket.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}

// 服务端
// 先启动服务端，再启动客户端
@Test
public void server() {
    ServerSocket ss = null;
    Socket socket = null;
    InputStream is = null;
    ByteArrayOutputStream baos = null;
    try {
        // 1.创建服务器端的ServerSocket，指明自己的端口号
        ss = new ServerSocket(8899);
        // 2.调用accept()表示接收来自于客户端的socket
        socket = ss.accept();
        // 3.获取输入流
        is = socket.getInputStream();

        // 不建议这样写，可能会有乱码，字节转字符
        //byte[] buffer = new byte[20];
        //int len;
        //while ((len = is.read(buffer)) != -1){
        //    String str = new String(buffer,0,len);
        //    System.out.print(str);
        //}
        // 4.读取输入流中的数据
        baos = new ByteArrayOutputStream();// 处理乱码问题
        byte[] buffer = new byte[20];
        int len;
        while ((len = is.read(buffer)) != -1){
            baos.write(buffer,0,len);
        }
        System.out.println(baos.toString());
        System.out.println("收到了来自于：" + socket.getInetAddress().getHostAddress() + "的数据");
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        // 5.关闭资源
        try {
            if (baos != null)
                baos.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
        try {
            if (is != null)
                is.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
        try {
            if (socket != null)
                socket.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
        try {
            if (ss != null)
                ss.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

例子2：客户端发送文件给服务端，服务端将文件保存在本地。

```Java
// 例子2：客户端发送文件给服务端，服务端将文件保存在本地
// 客户端
@Test
public void client() {
    Socket socket = null;
    OutputStream os = null;
    FileInputStream fis = null;
    try {
        // 1.创建Socket对象，指明服务器端的ip和端口号
        socket = new Socket(InetAddress.getByName("127.0.0.1"),8899);
        // 2.获取一个输出流，用于输出数据
        os = socket.getOutputStream();
        // 3.发送文件的操作
        fis = new FileInputStream(new File("miku.png"));
        
        byte[] buffer = new byte[5];
        int len;
        while ((len = fis.read(buffer)) != -1){
            os.write(buffer,0,len);
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        // 4.资源的关闭
        try {
            if (fis != null)
                fis.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
        try {
            if (os != null)
                os.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
        try {
            if (socket != null)
                socket.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

// 服务端
// 先启动服务端，再启动客户端
@Test
public void server() {
    ServerSocket ss = null;
    Socket socket = null;
    InputStream is = null;
    FileOutputStream fos = null;
    try {
        // 1.创建服务器端的ServerSocket，指明自己的端口号
        ss = new ServerSocket(8899);
        // 2.调用accept()表示接收来自于客户端的socket
        socket = ss.accept();
        // 3.获取输入流
        is = socket.getInputStream();

        // 4.读取输入流中的数据
        fos = new FileOutputStream(new File("test2.png"));
        byte[] buffer = new byte[5];
        int len;
        while ((len = is.read(buffer)) != -1){
            fos.write(buffer,0,len);
        }

        System.out.println("收到了来自于：" + socket.getInetAddress().getHostAddress() + "的数据");
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        // 5.关闭资源
        try {
            if (fos != null)
                fos.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
        try {
            if (is != null)
                is.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
        try {
            if (socket != null)
                socket.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
        try {
            if (ss != null)
                ss.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

注意：io流不要重复关闭。关闭顺序要求先开后，否则会报异常`“java.io.IOException:Stream Closed”`。

外层流关闭后内层流也会关闭。

例子3：客户端发送文件给服务端，服务端将文件保存在本地，并返回信息。

```Java
@Test
// 这里应该用try-catch-finally来处理异常，这里只是简单处理
public void client() throws IOException {
    //1.
    Socket socket = new Socket(InetAddress.getByName("127.0.0.1"),9090);
    //2.
    OutputStream os = socket.getOutputStream();
    //3.
    FileInputStream fis = new FileInputStream(new File("beauty.jpg"));
    //4.
    byte[] buffer = new byte[1024];
    int len;
    while((len = fis.read(buffer)) != -1){
        os.write(buffer,0,len);
    }
    //关闭数据的输出
    socket.shutdownOutput();// 必须加上这句，否则会陷入前面的循环出不来

    //5.接收来自于服务器端的数据，并显示到控制台上
    InputStream is = socket.getInputStream();
    ByteArrayOutputStream baos = new ByteArrayOutputStream();
    byte[] bufferr = new byte[20];
    int len1;
    while((len1 = is.read(buffer)) != -1){
        baos.write(buffer,0,len1);
    }

    System.out.println(baos.toString());

    //6.
    fis.close();
    os.close();
    socket.close();
    baos.close();
}
```

```Java
@Test
public void server() throws IOException {
    //1.
    ServerSocket ss = new ServerSocket(9090);
    //2.
    Socket socket = ss.accept();
    //3.
    InputStream is = socket.getInputStream();
    //4.
    FileOutputStream fos = new FileOutputStream(new File("beauty2.jpg"));
    //5.
    byte[] buffer = new byte[1024];
    int len;
    while((len = is.read(buffer)) != -1){
        fos.write(buffer,0,len);
    }

    System.out.println("图片传输完成");

    //6.服务器端给予客户端反馈
    OutputStream os = socket.getOutputStream();
    os.write("你好，照片我已收到，非常漂亮！".getBytes());

    //7.
    fos.close();
    is.close();
    socket.close();
    ss.close();
    os.close();

}
```

客户端与服务端：

客户端：自定义，浏览器

服务端：自定义，Tomcat服务器

总结：

![image-20210801214156325](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232125858.png)