---
title: Java入门笔记（六）
tags:
  - Java入门
  - 异常
categories:
  - Java
mathjax: true
copyright: true
abbrlink: 76cf8fe6
date: 2021-03-06 12:52:46
---

## Java异常机制（Exception）

<!--more-->

![image-20210306125425820](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232106787.png)

```java
//小例子
public class A {
    public static void main(String[] args) {
        new A().a();
    }
    public void a(){
        b();
    }
    public void b(){
        a();
    }
}
//Exception in thread "main" java.lang.StackOverflowError 栈溢出
```

![image-20210306130118928](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232107530.png)

![image-20210306130155892](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232107082.png)

### 错误（Error）与异常（Exception）

![image-20210306130317211](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232107429.png)

![image-20210306130400193](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232107360.png)

Exception可以分成运行时异常和非运行时异常。

从继承关系可知：`Throwable`是异常体系的根，它继承自`Object`。`Throwable`有两个体系：`Error`和`Exception`，`Error`表示严重的错误，程序对此一般无能为力，例如：

- `OutOfMemoryError`：内存耗尽
- `NoClassDefFoundError`：无法加载某个Class
- `StackOverflowError`：栈溢出

而`Exception`则是运行时的错误，它可以被捕获并处理。

某些异常是应用程序逻辑处理的一部分，应该捕获并处理。例如：

- `NumberFormatException`：数值类型的格式错误
- `FileNotFoundException`：未找到文件
- `SocketException`：读取网络失败

还有一些异常是程序逻辑编写不对造成的，应该修复程序本身。例如：

- `NullPointerException`：对某个`null`的对象调用方法或字段
- `IndexOutOfBoundsException`：数组索引越界

`Exception`又分为两大类：

1. `RuntimeException`以及它的子类；
2. 非`RuntimeException`（包括`IOException`、`ReflectiveOperationException`等等）

Java规定：

- 必须捕获的异常，包括`Exception`及其子类，但不包括`RuntimeException`及其子类，这种类型的异常称为Checked Exception。
- 不需要捕获的异常，包括`Error`及其子类，`RuntimeException`及其子类。

以上理论只作为了解内容，重点是如何处理异常。

### 异常处理（抛出与捕获）

![image-20210306130827800](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232107259.png)

所有异常都可以调用`printStackTrace()`方法打印异常栈，这是一个简单有用的快速打印异常的方法。

```java
public class Test {
    public static void main(String[] args) {
        int a = 1;
        int b = 0;
        //try监控区域
        try {
            System.out.println(a/b);
        }catch (ArithmeticException e){
            //catch用于捕获异常
            System.out.println("程序异常，变量b不能为0");
        }finally{
            //处理善后工作，无论是否异常都会执行
            System.out.println("finally");
        }
        //try-catch代码块是必要的
        //finally不是必须的
    }
}
```

```java
public class Test {
    public static void main(String[] args) {

        try{
            new Test().a();
        }catch (Throwable e){
            //catch(想要捕获的异常类型)
            System.out.println("Exception");
        }
    }
    public void a(){
        b();
    }
    public void b(){
        a();
    }
}
```

`try-catch...-catch`结构：一般把较大的异常放在后面。

假设要捕获多个异常，必须从小到大，否则会报错！

```java
public class Test {
    public static void main(String[] args) {
        int a = 1;
        int b = 0;

        try {
            System.out.println(a/b);
        }catch (Error e){
            //catch用于捕获异常
            System.out.println("Error");
        }catch (Exception e){
            System.out.println("Exception");
        }catch (Throwable t){
            System.out.println("Throwable");
        }
        finally{
            //处理善后工作，无论是否异常都会执行
            System.out.println("finally");
        }
        //try-catch代码块是必要的
        //finally不是必须的
    }
}
```

IDEA异常处理快捷键：

![image-20210306132935760](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232107505.png)

```java
public class Test {
    public static void main(String[] args) {
        try {
            new Test().test(1,0);
        } catch (ArithmeticException e) {
            e.printStackTrace();
        }

    }
    //假设这个方法处理不了这个异常，就从方法上抛出异常
    public void test(int a,int b) throws ArithmeticException{
        if (b == 0) {
            throw new ArithmeticException();//主动抛出异常，一般在方法中使用
        }
    }
}
```

![image-20210306135620960](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232107715.png)

**`throws`声明这个方法可能会抛出异常，`throw`语句是实际的抛出异常的语句。**

**异常处理的一般经验**：捕获那些你知道如何处理的异常，继续传播（抛出）那些不知道如何处理的异常。

### 自定义异常

大部分情况下，我们不需要自定义异常。

![image-20210306135807062](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232107917.png)

```java
//自定义的异常类
public class MyException extends Exception{
    //传递数字>10抛出异常
    private int detail;

    public MyException(int a) {
        this.detail = a;
    }
    //toString:异常的打印信息
    @Override
    public String toString() {
        return "MyException{" +
                "detail=" + detail +
                '}';
    }
}
public class Test {
    // 可能会存在异常的方法
    static void test(int a) throws MyException{
        System.out.println("传递的参数为："+a);
        if (a>10){
            throw new MyException(a);
        }
        System.out.println("OK");
    }

    public static void main(String[] args) {
        try {
            test(1);
            //传递的参数为：1 OK
            test(11);
            //传递的参数为：11 MyException=>MyException{detail=11}
        } catch (MyException e) {
            System.out.println("MyException=>"+e);
        }
    }
}
```

![image-20210306143205863](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232107084.png)

IDEA代码出现波浪线（一些异常等），按住Alt + Enter进行处理。
