---
title: Java入门笔记（十三）
tags:
  - Java入门
  - 注解
categories:
  - Java
mathjax: true
copyright: true
abbrlink: dffb7d20
date: 2021-05-02 21:06:29
---

## 1.BigInteger类和BigDecimal类

如果我们使用的整数范围超过了`long`型怎么办？这个时候，就只能用软件来模拟一个大整数。

<!--more-->

`java.math.BigInteger`就是用来表示任意大小的整数。`BigInteger`内部用一个`int[]`数组来模拟一个非常大的整数：

![image-20210502211408301](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232115676.png)

![image-20210502211423255](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232115380.png)

![image-20210502211432078](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232115027.png)

## 2.枚举类的使用

JDK1.5之前自定义枚举类了解就行。

- 重点掌握JDK1.5的新特性，enum关键字定义枚举类。

- 类的对象只有有限个，确定的。

- 当需要定义一组常量时，强烈建议使用枚举类。
- 若枚举只有一个对象 , 则可以作为一种**单例模式**的实现方式。

![image-20210503090139873](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232116781.png)

看看就行。

使用`enum`定义的枚举类是一种引用类型。前面我们讲到，引用类型比较，要使用`equals()`方法，如果使用`==`比较，它比较的是两个引用类型的变量是否是同一个对象。因此，引用类型比较，要始终使用`equals()`方法，但`enum`类型可以例外。

这是因为`enum`类型的每个常量在JVM中只有一个唯一实例，所以可以直接用`==`比较。

```Java
//enum关键字定义枚举类
public class SeasonTest {
    public static void main(String[] args) {
        Season summer = Season.SUMMER;
        System.out.println(summer);//SUMMER，无需重写toString
    }
}

//定义的枚举类默认继承于java.lang.Enum类
enum Season{
    //提供当前枚举类的对象，之间用","隔开，";"结束
    SPRING("春天"),//默认为private final
    SUMMER("夏天"),
    AUTUMN("秋天"),
    WINTER("冬天");
    //声明Season对象的私有属性
    private final String name;
    //私有化类的构造器，并给属性赋值
    private Season(String name){
        this.name = name;
    }
    //其他诉求，获取对象属性
    public String getName() {
        return name;
    }
}
```

枚举类可以应用在`switch`语句中。因为枚举类天生具有类型信息和有限个枚举常量，所以比`int`、`String`类型更适合用在`switch`语句中：

```Java
public class Main {
    public static void main(String[] args) {
        Weekday day = Weekday.SUN;
        switch(day) {
        case MON:
        case TUE:
        case WED:
        case THU:
        case FRI:
            System.out.println("Today is " + day + ". Work at office!");
            break;
        case SAT:
        case SUN:
            System.out.println("Today is " + day + ". Work at home!");
            break;
        default:
            throw new RuntimeException("cannot process " + day);
        }
    }
}

enum Weekday {
    SUN, MON, TUE, WED, THU, FRI, SAT; // 对应0~6
}
```

![image-20210503091556341](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232116549.png)

![image-20210503091617899](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232116133.png)

```Java
public static void main(String[] args) {
    Season[] values = Season.values();
    for (int i = 0;i < values.length;i++){
        System.out.println(values[i].toString());
    }
    System.out.println("____________________________");
    //valueOf(String objname)，返回枚举类中对象名是objname的对象
    Season winter = Season.valueOf("WINTER");
    System.out.println(winter);
}
/*SPRING
SUMMER
AUTUMN
WINTER
____________________________
WINTER*/
```

![image-20210503092548552](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232116607.png)

```Java
//每个枚举值分别实现同一个接口方法
public class SeasonTest {
    public static void main(String[] args) {
        Season season1 = Season.SUMMER;
        Season season2 = Season.SPRING;
        Season season3 = Season.AUTUMN;
        season1.show();
        season2.show();
        season3.show();
    }
/*summer
spring
autumn*/
}
interface Info{
    void show();
}
//定义的枚举类默认继承于java.lang.Enum类
enum Season implements Info{
    //提供当前枚举类的对象，之间用","隔开，";"结束
    SPRING("春天"){
        public void show(){
            System.out.println("spring");
        }
    },
    SUMMER("夏天"){
        public void show(){
            System.out.println("summer");
        }
    },
    AUTUMN("秋天"){
        public void show(){
            System.out.println("autumn");
        }
    },
    WINTER("冬天"){
        public void show(){
            System.out.println("winter");
        }
    };
    //声明Season对象的四有属性
    private final String name;
    //私有化类的构造器，并给属性赋值
    private Season(String name){
        this.name = name;
    }
    //其他诉求，获取对象属性
    public String getName() {
        return name;
    }
}
```

## 3.注解 (Annotation）

![image-20210503093537140](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232116349.png)

一定程度上可以说： **框架 = 注解 + 反射 + 设计模式**。

![image-20210503093702587](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232116530.png)

**自定义Annotation**：

![image-20210503094509895](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232116660.png)

![image-20210503094431440](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232116755.png)

没有成员定义的 Annotation 称为 **标记** ，包含成员变量的 Annotation 称为**元数据** Annotation

```Java
//注解声明为@interface
public @interface MyAnnotation {
    //内部定义成员，通常使用value表示
    //可以指定成员的默认值，使用default定义
    //自定义注解必须使用反射才有意义
    String value() default "string";

}
```

元注解：注解其他注解的注解

![image-20210503095742361](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232116546.png)

![image-20210503095819473](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232116226.png)

默认为CLASS。![image-20210503095906860](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232116915.png)

![image-20210503095953225](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232117967.png)

**自定义注解一般会指明以上两个元注解：Retention、Target。**

下面两个用到频率较低。

![image-20210503100238905](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232117651.png)

Java 8对注解处理提供了两点改进： **可重复的注解** 及 **可用于类型的注解** 。此外，反射也得到了加强，在 

Java8 中能够得到方法参数的名称。这会简化标注在方法参数上的注解。

![image-20210503101428719](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232117006.png)