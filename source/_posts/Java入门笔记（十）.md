---
title: Java入门笔记（十）
tags:
  - Java入门
categories:
  - Java
mathjax: true
copyright: true
abbrlink: ac5bd4a1
date: 2021-04-04 13:04:00
---

## 1.包装类 (Wrapper) 的使用

<!--more-->

![image-20210404124237876](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232110249.png)

![image-20210404132608185](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232110284.png)

所有的包装类型都是不变类。我们查看`Integer`的源码可知，它的核心代码如下：

```java
public final class Integer {
    private final int value;
}
```

因此，一旦创建了`Integer`对象，该对象就是不变的。

对两个`Integer`实例进行比较要特别注意：绝对不能用`==`比较，因为`Integer`是引用类型，必须使用`equals()`比较。

包装类的重点就在于基本数据类型、包装类和String类之间的转换。

**重点记住：自动装/拆箱，parseXXX()/valueOf()。**

注意：自动装箱和自动拆箱只发生在编译阶段，目的是为了少写代码。

装箱和拆箱会影响代码的执行效率，因为编译后的`class`代码是严格区分基本类型和引用类型的。

```java
// 基本数据类型-->包装类：调用包装类的构造器
@org.junit.jupiter.api.Test
    public void test1(){
    int num1 = 10;
    Integer in1 = new Integer(num1);
    System.out.println(in1.toString());

    Integer in2 = new Integer("1234");
    System.out.println(in2);// 输出对象默认调用toString

    Float f1 = new Float("123.34F");
    System.out.println(f1);
    Float f2 = new Float("234");
    System.out.println(f2);

    Boolean b1 = new Boolean(true);//true
    System.out.println(b1);
    Boolean b2 = new Boolean("TRue12");//false，不会报错
    System.out.println(b2);
    Boolean b3 = new Boolean("TRue");//true，忽略大小写
    System.out.println(b3);
}
```

```java
// 包装类-->基本数据类型：调用包装类的xxxValue()
@Test
public void test2(){
    Integer in1 = new Integer(12);
    int i1 = in1.intValue();
    System.out.println(i1%5);
}
```

```java
// JDK5.0新特性，自动装箱与自动拆箱
    @Test
    public void test3(){
        int num1 = 10;
        // 基本数据类型-->包装类的对象
        method(num1);
        // 自动装箱：基本数据类型-->包装类
        //不需要调用包装类的构造器
        int num2 = 18;
        Integer in1 = num2;

        // 自动拆箱：包装类-->基本数据类型
        int num3 = in1;
        System.out.println(num3);
    }
    public void method(Object obj){
        System.out.println(obj);
    }
```

```java
// 基本数据类型、包装类-->String类型：调用String重载的valueOf()
@Test
public void test4(){
    int num1 = 10;
    // 方式1：连接运算
    String s1 = num1+"";
    System.out.println(s1);
    // 方式2
    float f1 = 12.3F;
    String s2 = String.valueOf(f1);//基本数据类型
    System.out.println(s2);

    Double d1 = new Double(12.4);//包装类
    String s3 = String.valueOf(d1);
    System.out.println(s3);
}
```

```java
// String类型-->基本数据类型、包装类：调用包装类的parseXXX()
@Test
public void test5(){
    String s1 = "123";
    int num1 = Integer.parseInt(s1);
    System.out.println(num1+3);

    String s2 = "true";
    boolean num2 = Boolean.parseBoolean(s2);
    System.out.println(num2);
}
```

```java
//关于包装类使用的面试题
// 问test1和test2输出是否相等
// 问test3的输出内容
public class InterviewTest {
	@Test
	public void test1() {
        // 三元运算符要求后面两个返回值类型一致，否则自动类型转换
		Object o1 = true ? new Integer(1) : new Double(2.0);
		System.out.println(o1);// 1.0
	}

	@Test
	public void test2() {
		Object o2;
		if (true)
			o2 = new Integer(1);
		else
			o2 = new Double(2.0);
		System.out.println(o2);// 1

	}

	@Test
	public void test3() {
		Integer i = new Integer(1);
		Integer j = new Integer(1);
		System.out.println(i == j);//false
/*解释：
三目运算符比较基本数据类型，所以在编译阶段自动拆箱为 int 和 double 类型，由于三目运算符要求 表达式2 和 表达式3 类型一致，所以在编译阶段自动类型提升（即 int 自动类型转换为 double 类型），再自动装箱为Object，输出时使用多态调用重写的toString();即Double包装类的toString();*/
------------------------------------------------------------------------
//Integer内部定义了IntegerCache结构，IntegerCache中定义了Integer[],
//保存了从-128~127范围的整数。如果我们使用自动装箱的方式，给Integer赋值的范围在
//-128~127范围内时，可以直接使用数组中的元素，不用再去new了。目的：提高效率		
		Integer m = 1;
		Integer n = 1;
		System.out.println(m == n);//true

		Integer x = 128;//相当于new了一个Integer对象
		Integer y = 128;//相当于new了一个Integer对象
		System.out.println(x == y);//false
	}

}
```

补充知识：搞懂Java集合类，参照[文章](https://mp.weixin.qq.com/s/8EeWLdK7UZt81sGpQZvh9A)，更多内容看公众号。

## 2.单例（Singleton）设计模式

什么是设计模式？

设计 模式 是在大量的实践中总结和理论化之后优选的代码结构、编程风格、以及解决问题的思考方式 。 设

计模免去我们自己再思考和摸索 。就 像是经典的棋谱，不同的棋局，我们用不同 的 棋谱。

简而言之，就两字，**“套路”**。

设计模式和算法一样，也是语言无关的。用Java较多。

入门可以参考大话设计模式。

所谓 类的**单例设计模式**，就是采取一定的方法保证在整个的软件系统中，对某个类 只能存在**一个对象实例** ，

并且该类只提供**一个**取得其对象实例的方法。

如果我们要让类在一个虚拟机中只能产生一个对象，我们首先必须将类的**构造器的访问权限设置为 private** ，这样，就不能用 new 操作符在类的外部产生类的对象了，但在类内部仍可以产生该类的对象。因为在类的外部开始还无法得到类的对象，只能调用**该类的某个静态方法**以返回类内部创建的对象，静态方法只能访问类中的静态成员变量，所以，指向类内部产生的 **该类对象的变量也必须定义成静态的** 。

**单例模式分为两种：饿汉式和懒汉式。**

```Java
public class Singleton {
    public static void main(String[] args) {
        Bank bank1 = Bank.getInstance();
        Bank bank2 = Bank.getInstance();
        System.out.println(bank1 == bank2);

        Order order1 = Order.getInstance();
        Order order2 = Order.getInstance();
        System.out.println(order1 == order2);
    }
}

// 饿汉式
class Bank{
    // 1.私有化构造器
    private Bank(){}

    // 2.内部创建类的对象，必须是静态的
    private static Bank instance = new Bank();

    // 3.提供公有静态方法，返回类的对象
    public static Bank getInstance(){
        return instance;
    }
}

// 懒汉式
class Order{
    // 1.私有化构造器
    private Order(){}

    // 2.声明当前类对象，没有初始化，必须是静态的
    private static Order instance = null;

    // 3.提供公有静态方法，返回类的对象
    public static Order getInstance(){
        if (instance == null){
            instance = new Order();
        }
        return instance;
    }
}
```

**对比**

1. 饿汉式：

坏处：对象加载时间过长。

好处：饿汉式是线程安全的。

2. 懒汉式：

好处：延迟对象的创建。

目前的懒汉式写法坏处：线程不安全。—>到多线程内容时，再修改

![image-20210502094523609](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232110311.png)