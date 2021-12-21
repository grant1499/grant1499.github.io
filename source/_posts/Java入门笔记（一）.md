---
title: Java入门笔记（一）
tags:
  - Java入门
categories:
  - Java
mathjax: true
copyright: true
abbrlink: b67a5797
date: 2021-02-24 07:04:32
---

## 1.java简介

本教程推荐配合大佬[笔记](https://www.cnblogs.com/vccyb/tag/%E5%8C%85%E5%AD%90%E5%AD%A6%E7%B3%BB%E5%88%97/)食用！

B站视频倍速播放：F12-->console-->输入：`document.querySelector('video').playbackRate = 2.5`，控制倍速。

随着Java的发展，SUN给Java又分出了三个不同版本：

- Java SE：Standard Edition
- Java EE：Enterprise Edition
- Java ME：Micro Edition

<!--more-->

推荐教程：[廖雪峰Java教程](https://www.liaoxuefeng.com/wiki/1252599548343744)

[java知识点总结（面经）](https://www.cyc2018.xyz/Java/)

[java基础知识总结](https://snailclimb.gitee.io/javaguide/#/docs/java/basis/java%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E6%80%BB%E7%BB%93)

```
┌───────────────────────────┐
│Java EE                    │
│    ┌────────────────────┐ │
│    │Java SE             │ │
│    │    ┌─────────────┐ │ │
│    │    │   Java ME   │ │ │
│    │    └─────────────┘ │ │
│    └────────────────────┘ │
└───────────────────────────┘
```

- JDK：Java Development Kit（java开发工具包）
- JRE：Java Runtime Environment(Java运行环境)
- 简单而言，使用JDK的开发工具完成的java程序，交给JRE去运行。

简单地说，JRE就是运行Java字节码的虚拟机。但是，如果只有Java源码，要编译成Java字节码，就需要JDK，因为JDK除了包含JRE，还提供了编译器、调试器等开发工具。

JDK:**JRE + 开发工具集**（编译器(Javac) + 其他工具）；JRE:**Java虚拟机(JVM) + Java类库** + 其他。

二者关系如下：

```
 ┌─    ┌──────────────────────────────────┐
 │     │     Compiler, debugger, etc.     │
 │     └──────────────────────────────────┘
JDK ┌─ ┌──────────────────────────────────┐
 │  │  │                                  │
 │ JRE │      JVM + Runtime Library       │
 │  │  │                                  │
 └─ └─ └──────────────────────────────────┘
       ┌───────┐┌───────┐┌───────┐┌───────┐
       │Windows││ Linux ││ macOS ││others │
       └───────┘└───────┘└───────┘└───────┘
```

- java：这个可执行程序其实就是JVM，运行Java程序，就是启动JVM，然后让JVM执行指定的编译后的代码；
- javac：这是Java的编译器，它用于把Java源码文件（以`.java`后缀结尾）编译为Java字节码文件（以`.class`后缀结尾）；
- jar：用于把一组`.class`文件打包成一个`.jar`文件，便于发布；
- javadoc：用于从Java源码中自动提取注释并生成文档；
- jdb：Java调试器，用于开发阶段的运行调试。

如果遇到编码问题，我们可以使用 **-encoding** 选项设置 **utf-8** 来编译：

```
javac -encoding UTF-8 HelloWorld.java 
java HelloWorld 
```



## 2.Java与C++的区别

我知道很多人没学过 C++，但是面试官就是没事喜欢拿咱们 Java 和 C++ 比呀！没办法！！！就算没学过 C++，也要记下来！

- 都是面向对象的语言，都支持封装、继承和多态
- Java 不提供指针来直接访问内存，程序内存更加安全
- Java 的类是单继承的，C++ 支持多重继承；虽然 Java 的类不可以多继承，但是接口可以多继承。
- Java 有自动内存管理垃圾回收机制(GC)，不需要程序员手动释放无用内存
- **在 C 语言中，字符串或字符数组最后都会有一个额外的字符`'\0'`来表示结束。但是，Java 语言中没有结束符这一概念。** 这是一个值得深度思考的问题，具体原因推荐看这篇文章： https://blog.csdn.net/sszgg2006/article/details/49148189

## 3.java语言编译(compile)与解释并存

Java 语言既具有编译型语言的特征，也具有解释型语言的特征，因为 <u>Java 程序要经过先编译，后解释两个步骤</u>，由 Java 编写的程序需要先**经过编译步骤，生成字节码**（*.class 文件），这种字节码必须**由 Java 解释器来解释执行**。因此，我们可以认为 Java 语言编译与解释并存。

## 4.字符型常量和字符串常量的区别

1. 形式上: 字符常量是单引号引起的一个字符; 字符串常量是双引号引起的 0 个或若干个字符

2. 含义上: 字符常量相当于一个整型值( ASCII 值),可以参加表达式运算; 字符串常量代表一个地址值(该字符串在内存中存放位置)

3. 占内存大小 字符常量只占 2 个字节; 字符串常量占若干个字节 (**注意： char 在 Java 中占两个字节**),

   > 字符封装类 `Character` 有一个成员常量 `Character.SIZE` 值为 16,单位是`bits`,该值除以 8(`1byte=8bits`)后就可以得到 2 个字节

## 5.Java注释

主要分为三种：单行、多行和文档注释。

```java
public class HelloWorld {
   /** 这是第一个Java程序
    * 它将输出 Hello World
    * 这是一个文档的示例
    */
    public static void main(String[] args){
       // 这是单行注释的示例
       /* 
       多行注释
       */
       System.out.println("Hello World"); 
    }
}
```

## 6.Java关键字

| 访问控制             | private  | protected  | public   |              |            |           |        |
| -------------------- | -------- | ---------- | -------- | ------------ | ---------- | --------- | ------ |
| 类，方法和变量修饰符 | abstract | class      | extends  | final        | implements | interface | native |
|                      | new      | static     | strictfp | synchronized | transient  | volatile  |        |
| 程序控制             | break    | continue   | return   | do           | while      | if        | else   |
|                      | for      | instanceof | switch   | case         | default    |           |        |
| 错误处理             | try      | catch      | throw    | throws       | finally    |           |        |
| 包相关               | import   | package    |          |              |            |           |        |
| 基本类型             | boolean  | byte       | char     | double       | float      | int       | long   |
|                      | short    | null       | true     | false        |            |           |        |
| 变量引用             | super    | this       | void     |              |            |           |        |
| 保留字               | goto     | const      | null     |              |            |           |        |

## 7.java语法规范

编写 Java 程序时，应注意以下几点：

- **大小写敏感**：Java 是大小写敏感的，这就意味着标识符 Hello 与 hello 是不同的。
- **类名**：（class）对于所有的类来说，类名的首字母应该大写。如果类名由若干单词组成，那么每个单词的首字母应该大写，例如 **MyFirstJavaClass** 。
- **方法名**：（method）所有的方法名都应该以小写字母开头。如果方法名含有若干单词，则后面的每个单词首字母大写。
- **源文件名**：源文件名必须和类名相同。当保存文件的时候，你应该使用类名作为文件名保存（切记 Java 是大小写敏感的），文件名的后缀为 **.java**。（如果文件名和类名不相同则会导致编译错误）。
- **主方法入口**：所有的 Java 程序由 **public static void main(String[] args)** 方法开始执行。

像其他语言一样，Java可以使用修饰符来修饰类中方法和属性。主要有两类修饰符：

1. 访问控制修饰符 : default, public , protected, private
2. 非访问控制修饰符 : final, abstract, static, synchronized

- **标识符命名规范**：

  所有的标识符都应该以英文字母、$、或者_开始。

  首字符之后可以是英文字母、$_或者数字`的任意组合。

  ![image-20210210101349712](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232122048.png)

  

**小结**

一个Java源码只能定义一个`public`类型的class，并且class名称和文件名要完全一致；

但是可以定义多个class（类）;

使用`javac`可以将`.java`源码编译成`.class`字节码；（JVM运行字节码）

使用`java`可以运行一个已编译的Java程序，参数是类名。

## 8.java变量与数据类型

Java变量就好像一个容器，可以保存程序在运行过程中的值，它在声明的时候会定义对应的数据类型（Java分为两种数据类型：基本数据类型和引用数据类型）。变量按作用域的范围又可分为三种类型：**局部变量，成员变量和静态变量**。

Java中并没有全局变量的概念，这与C/C++不同！！

```java
public class Variable{
    static int allClicks = 0;// 类变量，静态变量
    String s = "helloworld";// 实例变量，成员变量
    public void method(){
        int i = 0;// 局部变量
    }
}
```



##### 8.1 主要分类

![image-20210210102225703](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232122079.png)

八大基本数据类型

- 1字节 的是 byte 、boolean
- 2字节 的是 short 、**char**（Java采用**Unicode编码**）
- 4字节 的是 int （范围约21亿=2$^{31}$）、float
- 8字节 的是 long 、double

```java
long num1 = 30L;
long num1 = 30l;// 两者等价
float num2 = 23.1F;
float num2 = 23.1f;// 两者等价
double = 23.23453456;// double不需要后缀，Java中的小数默认为double型
char c = 'A';
Srting s = "ABC";// 注意：String是一个类，不是关键字，不属于基本数据类型
boolean flag = true;
```

##### 8.2 局部变量

声明局部变量时的注意事项：

- 局部变量声明在方法，构造方法或语句块中。
- 局部变量在方法，构造方法，或者语句块被执行的时候创建，当其执行完成后，将会被销毁。
- 访问修饰符不能用于局部变量。
- 局部变量只在声明它的方法，构造方法或语句块中可见。
- 局部变量是在栈上分配的。
- 局部变量没有默认值，所以局部变量被声明后，**必须经过初始化，才可以使用**。

##### 8.3 成员变量

在类内部但在方法体外声明的变量称为成员变量，或者实例变量。之所以称为实例变量，是因为该变量只能通过类的实例（对象）来访问。

```java
/** 文档注释：
 * @author 微信搜「沉默王二」，回复关键字 PDF
 */
public class InstanceVariable {
    int data = 88;// data是成员变量
    public static void main(String[] args) {
        InstanceVariable iv = new InstanceVariable();
        System.out.println(iv.data); // 88
    }
}
```

可以是一个变量，它是一个引用类型的变量。`new`关键字可以创建一个类的实例（也称为对象），通过“ =”操作符赋值给iv这个变量，iv就成了这个对象的引用，通过`iv.data`就可以访问成员变量了。

声明成员变量时的注意事项：

- 成员变量声明在一个类中，但在方法，构造方法和语句块之外。
- 当一个对象被实例化之后，每个成员变量的值就跟着确定。
- 成员变量在对象创造的时候创造，在对象被销毁的时候销毁。
- 成员变量的值应该至少被一个方法，构造方法或语句块引用，以便外部能够通过这些方式获取实例变量信息。
- 成员变量可以声明在使用前或使用后。
- 访问修饰符可以修饰成员变量。
- 一般情况下应该把成员变量设为所有权。通过使用访问修饰符可以使成员变量对子类可见；成员变量具有唯一值。数值型成员变量对于类中的方法，构造方法或语句块是可见的。变量的默认值是0，布尔型变量的默认值是false，引用类型变量的默认值是null。变量的值可以在声明时指定，也可以在构造方法中指定。

## 9.位运算
```java
>>：符号位不动，其余位右移，符号位后边正数补0，负数补1。又称带符号右移
>>>：符号位一起右移，左边补0，又称无符号右移
<<：左移，右边补0。左移没有带符号位一说，因为符号位在最左侧
```

## 10.关于cmd中文乱码的解决方案

```shell
F:\Java_VScode>javac -encoding utf-8 Calculation.java # 加上一个编码参数
F:\Java_VScode>java Calculation
2 7
最大公约数：1
```

文件是以UTF-8的形式保存的。

这样做保证javac和java的字符集统一为UTF-8，就不会出现中文乱码。

在VScode通过右键code-Runner编译时发现这样做还是乱码。

![image-20210723082254496](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232122110.png)

需要修改powershell的编码格式为UTF-8，才能保证格式统一。

还是通过main方法上面的run来运行。