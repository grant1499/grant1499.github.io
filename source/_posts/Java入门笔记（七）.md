---
title: Java入门笔记（七）
tags:
  - Java入门
categories:
  - Java
mathjax: true
copyright: true
abbrlink: a4cff879
date: 2021-04-03 11:43:13
---

## 1.方法和属性的可见性修饰符

<!--more-->

参考博客： https://www.cnblogs.com/starhu/p/5143983.html。（总结全面！）

![image-20210403114450929](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232108403.png)

- 非公共的类，不能在包外被使用。

- public的方法类似一种约定，既然外面的代码可以使用，就意味着不能乱改。比如签名不能改之类的。
- 对于private的方法，因为类外面调不到，所以无论怎么改，也不会影响（直接影响）类外面的代码。

- 有些时候，会把所有的构造方法都定义成private的，然后使用静态方法调用构造方法，这样的好处是可以通过代码，检查每个属性值是否合法。

```java
public static MerchandiseV2 createMerchandise(String name, String id, int count,
                                                  double soldPrice, double purchasePrice) {
        if (soldPrice < 0 || purchasePrice < 0) {
            return null;
        }
        return new MerchandiseV2(name, id, count, soldPrice, purchasePrice);// 合法则调用构造器
    }    
```

|   访问控制修饰符   | 本类内部 | 同包 | 不同包子类 | 同一工程 |
| ---- | ---- | ---- | ---- | ---- |
| Public | 可以 | 可以 | 可以 | 可以 |
| Protected | 可以 | 可以 | 可以 |      |
| 默认（default） | 可以 | 可以 |      |      |
| private | 可以 | | | |

**注意，上述四种访问权限，只有默认访问权限和public能够用来修饰类。修饰类的变量和方法四种权限都可以。（本处所说的类针对的是外部类，不包括内部类）**

## 2.Math类

Java 的 Math 包含了用于执行基本数学运算的属性和方法，如初等指数、对数、平方根和三角函数。

Math类中的方法都是静态的，所以不需要实例化对象。

它没有成员变量，是一个工具类，只提供一些方法。

```java
Math.abs(-10) // 绝对值
Math.sqrt(X)//计算平方根
Math.cbrt(X)//计算立方根
Math.pow(a, b)//计算a的b次方
Math.max( 1,2 );//计算最大值
Math.min( 3,4 );//计算最小值
Math.ceil(X) //接近此数的大的整数的值 3.1->4
Math.floor(X) //接近此数的小的整数的值 3.1->3
Math.random() //[0,1)
Math.round(X) //round 四舍五入，float时返回int值，double时返回long值
   
Math.random(); // 0.53907... 每次都不一样，返回值是double
//前面我们使用的Math.random()实际上内部调用了Random类
//random在java.util包里
```

```java
import java.util.Random;
public class Application {
    public static void main(String[] args) {
        System.out.println(Math.random());
        Random random = new Random();
        for (int i = 0;i < 5;i++){
            //nextInt的返回值竟然有正数有负数哦！随机生成一个int值
            System.out.println(Math.abs(random.nextInt()));
        }

        System.out.println(Math.round(-9.2));
        System.out.println(Math.round(-9.5));
        System.out.println(Math.round(-9.8));
        System.out.println(Math.round(9.2));
        System.out.println(Math.round(9.5));
        System.out.println(Math.round(9.8));
    }
}
/*0.8715393731162016
1876651818
1246645657
1659850256
1890712276
1135262753
-9
-9
-10
9
10
10*/
```

## 3.重新认识String类

String 对象最重要的特点：**不可变（immutable）**不可变不可变，重要的事情说三遍。String 用来存储字符的数据是private的，而且不提供任何修改内容的方法，**所以String 对象一旦生成，其内容就是完全不可能被修改的。**

这种不可变性是通过内部的`private final char[]`字段，以及没有任何修改`char[]`的方法实现的。

String是一个final类，不可变。

String 对象的字符内容是存储在一个字符数组 value 中的。

![image-20210502134230914](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232108410.png)

![image-20210502134630605](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232108607.png)

![image-20210502134822887](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232108782.png)

`String s = new String("abc");`方式创建对象，在内存中创建了几个对象？
**两个**:一个是堆空间中new结构，另一个是char[]对应的常量池中的数据：`"abc"`

```java
public class LearnString {
    public static void main(String[] args) {
        String content = "01234567ABCDefgh";// java为String开后门，不用new String创建
        // String的length()是个方法不是属性哦，数组的length是属性
        System.out.println(content.length());

        // 其实是生成了一个新的String对象
        System.out.println(content.toUpperCase());//全部大写，生成副本
        System.out.println(content.toLowerCase());//全部小写，生成副本

        // content指向对象的内容并没有变化
        System.out.println(content);
        //返回指定索引处的字符。索引范围为从 0 到 length() - 1
        // Java的String索引不像C++可以用[]提取
        System.out.println(content.charAt(1));

        // System.out.println(content.charAt(99));
        System.out.println(content.substring(5));
        System.out.println(content.substring(1, 5));
    }
}
/*16
01234567ABCDEFGH
01234567abcdefgh
01234567ABCDefgh
1
567ABCDefgh
1234*/
```

`substring(int beginIndex，int endIndex)` 形式：左闭右开

此方法中的 `beginIndex` 表示截取的起始索引，截取的字符串中包括起始索引对应的字符；`endIndex` 表示结束索引，截取的字符串中不包括结束索引对应的字符，如果不指定 `endIndex`，则表示截取到目标字符串末尾。

```java
public class LearnString2 {
    public static void main(String[] args) {
        String content = "Orange_Apple_Banana";
	//toCharArray() 方法将字符串转换为字符数组
        char[] chars = content.toCharArray();
        for (int i = 0; i < chars.length; i++) {
            System.out.println(chars[i]);
        }

        String sp = "_";
        String[] s = content.split(sp);// 分割String，用char数组保存
        for (int i = 0; i < s.length; i++) {
            System.out.println(s[i]);
        }
        
        String s1 = "  ad ";
        String s2 = s1.trim();//去除左右两边的空格
        String s3 =new String("abc");
        System.out.println(s1.compareTo(s2));//比较String大小
        System.out.println(s1.compareTo(s3));

        int indexOf = content.indexOf('_');
        System.out.println(indexOf);
        System.out.println(content.substring(indexOf + 1, content.length()));

        int lastIndexOf = content.lastIndexOf("_");
        System.out.println(lastIndexOf);
        System.out.println(content.substring(0, lastIndexOf));

        System.out.println(content.contains("apple"));
        System.out.println(content.contains("Apple"));
        System.out.println(content.startsWith("Orange"));
        System.out.println(content.endsWith("Banana"));
        
        String content2 = "Orange_Apple_Banana";
        String content3 = "   orange_Apple_banana   ";

        // TODO 两个String对象比较是否相等，一定要用equals方法
        System.out.println(content.equals(content2));
        System.out.println(content.equals(content3));
        System.out.println(content.equalsIgnoreCase(content3.trim()));//忽略大小写
    }
}
```

`indexOf()` 方法有以下四种形式：

- **int indexOf(int ch)/int indexOf(String str):** 返回指定字符（索引）或字符串在字符串中第一次出现处的索引，如果此字符串中没有这样的字符，则返回 -1。
- **int indexOf(int ch, int fromIndex)/int indexOf(String str, int fromIndex):** 返回从 `fromIndex` 位置开始查找指定字符（索引）或字符串在字符串中第一次出现处的索引，如果此字符串中没有这样的字符，则返回 -1。

`lastIndexOf()` 方法也有四种形式：

- 前两种把第一次改成最后一次就行；
- 后两种改为从 `fromIndex` 位置反向查找即可。

`startsWith()`和`endsWith()` 方法用于测试字符串是否以指定的字符串前/后缀结束。

`contains()` 方法用于判断字符串中是否包含指定的字符或字符串。

**equals方法：**

`equals()` 方法用于将字符串与指定的对象比较。

String 类中重写了 `equals()` 方法用于比较两个字符串的内容是否相等。

如果给定对象与字符串相等，则返回 true；否则返回 false。

**注意：String 中 == 比较引用地址是否相同，equals() 比较字符串的内容是否相同。**

用==进行比较时，符号两边的数据类型必须兼容，可自动转换的基本数据类型除外，否则编译出错。

`equalsIgnoreCase()` 方法用于将字符串与指定的对象比较，不考虑大小写。

`contains()` 方法用于判断字符串中是否包含指定的字符或字符串。

`trim()` 方法用于删除字符串的头尾空白符。

**String类的类型转换：**在入门笔记（十）中的包装类中有详细讲解

要把任意基本类型或引用类型转换为字符串，可以使用静态方法`valueOf()`。这是一个重载方法，编译器会根据参数自动选择合适的方法：

```java
String.valueOf(123); // "123"
String.valueOf(45.67); // "45.67"
String.valueOf(true); // "true"
String.valueOf(new Object()); // 类似java.lang.Object@636be97c
```

## 4.String类与char[]、byte[]之间的转换

![image-20210502142039420](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232108903.png)

如果修改了`char[]`数组，`String`并不会改变：

这是因为通过`new String(char[])`创建新的`String`实例时，它并不会直接引用传入的`char[]`数组，而是会复制一份，所以，修改外部的`char[]`数组不会影响`String`实例内部的`char[]`数组，因为这是两个不同的数组。

从`String`的不变性设计可以看出，如果传入的对象有可能改变，我们需要复制而不是直接引用。

```Java
public class StringTest {
    public static void main(String[] args) {
        String s1 = "abc1234";
        char[] c1 = s1.toCharArray();
        for (int i = 0;i < c1.length;i++){
            System.out.println(c1[i]);
        }

        char[] c2 = {'h','e','l','l','o'};
        String s2 = new String(c2);
        System.out.println(s2);
    }
}
```

![image-20210502142755814](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232108177.png)

```Java
public class StringTest {
    public static void main(String[] args) {
        String s1 = "abc1234";
        byte[] b1 = s1.getBytes();// 可以在getBytes中指定编码集
        System.out.println(Arrays.toString(b1));// 输出UTF-8编码，默认编码集

        String s2 = new String(b1);
        System.out.println(s2);
    }
}
```

