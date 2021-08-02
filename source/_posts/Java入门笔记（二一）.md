---
title: Java入门笔记（二一）
tags:
  - Java入门
  - 反射
categories:
  - Java
mathjax: true
copyright: true
abbrlink: 5b571d4d
date: 2021-07-31 16:30:39
---

## 8.调用运行时类的指定结构（重点）

主要是属性和方法，构造器。属性和**方法**更常用。

<!--more-->

首先是调用属性。

提示：第一个方法不常用，开发常用第二个方法。

```Java
// 调用运行时类的指定结构：属性、方法、构造器
@Test
public void test5() throws Exception {
    Class clazz = Person.class;
    // 创建运行时类的对象
    Person p = (Person)clazz.newInstance();
    // 获取指定的属性，要求运行时类的属性声明为public
    // 通常不太用
    Field age = clazz.getField("age");
    // 设置当前属性的值，set：参数1，指明设置对象的哪个属性，参数2，指明属性值设为多少
    age.set(p,12);
    // 获取当前属性的值，get：指明获取对象的哪个属性
    int pAge = (int)age.get(p);
    System.out.println(pAge);
}
// 获取指定的属性的常用做法，开发中常用
@Test
public void test6() throws Exception {
    Class clazz = Person.class;
    // 创建运行时类的对象
    Person p = (Person)clazz.newInstance();
    // 1.获取运行时类中指定变量名的属性，私有属性也能拿到
    Field name = clazz.getDeclaredField("name");
    // 2.保证当前属性是可访问的
    name.setAccessible(true);
    name.set(p,"Newton");
    System.out.println(name.get(p));
}
```

然后是方法。

```Java
// 如何操作运行时类中的指定方法
@Test
public void testMethod() throws Exception {
    Class clazz = Person.class;
    //  创建运行时类的对象
    Person p = (Person)clazz.newInstance();
    //  1.获取指定的某个方法，private String showNation(String Nation)
    //  getDeclaredMethod：参数1，指明获取的方法的名称，参数2，指明获取的方法的形参列表
    Method showNation = clazz.getDeclaredMethod("showNation", String.class);
    // 2.保证当前方法是可访问的
    showNation.setAccessible(true);
    // 3.通过invoke执行方法
    //  invoke：参数1，方法的调用者，参数2，给方法形参赋值得到实参
    // invoke的返回值即为调用的方法的返回值，没有返回值返回null
    Object chn = showNation.invoke(p, "CHN");// 默认为Object，可以强转
    System.out.println(chn);

    // 如何调用静态方法，private static void info()
    Method info = clazz.getDeclaredMethod("info");
    info.setAccessible(true);
    info.invoke(Person.class);
    // info.invoke(null);这样也行，任意对象都能调用静态方法

}
```

最后是构造器。

```Java
// 如何调用运行时类的指定的构造器，不太常用
@Test
public void test7() throws Exception {
    Class clazz = Person.class;

    // private Person(String name)
    // 1.获取指定的构造器
    // 参数：指明构造器的参数列表
    Constructor constructor = clazz.getDeclaredConstructor(String.class);
    // 2.保证当前构造器是可访问的
    constructor.setAccessible(true);
    // 3.调用此构造器创建运行时类的对象
    Person p = (Person)constructor.newInstance("Tom");
    System.out.println(p);
}
```

JavaSE阶段回顾总结：

https://www.bilibili.com/video/BV1MJ411v7tJ