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

## 1.调用运行时类的指定结构（重点）

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

## 2.集合框架内容补充

### Queue

队列（`Queue`）是一种经常使用的集合。`Queue`实际上是实现了一个先进先出（FIFO：First In First Out）的有序表。它和`List`的区别在于，`List`可以在任意位置添加和删除元素，而`Queue`只有两个操作：

- 把元素添加到队列末尾；
- 从队列头部取出元素。

在Java的标准库中，队列接口`Queue`定义了以下几个方法：

- `int size()`：获取队列长度；
- `boolean add(E)`/`boolean offer(E)`：添加元素到队尾；
- `E remove()`/`E poll()`：获取队首元素并从队列中删除；
- `E element()`/`E peek()`：获取队首元素但并不从队列中删除。

|                    | throw Exception | 返回false或null    |
| :----------------- | :-------------- | ------------------ |
| 添加元素到队尾     | add(E e)        | boolean offer(E e) |
| 取队首元素并删除   | E remove()      | E poll()           |
| 取队首元素但不删除 | E element()     | E peek()           |

因此，两套方法可以根据需要来选择使用。

注意：不要把`null`添加到队列中，否则`poll()`方法返回`null`时，很难确定是取到了`null`元素还是队列为空。

```java
public class Main {
    public static void main(String[] args) {
        Queue<String> q = new LinkedList<>();
        // 添加3个元素到队列:
        q.offer("apple");
        q.offer("pear");
        q.offer("banana");
        // 从队列取出元素:
        System.out.println(q.poll()); // apple
        System.out.println(q.poll()); // pear
        System.out.println(q.poll()); // banana
        System.out.println(q.poll()); // null,因为队列是空的
    }
}
```

### PriorityQueue

`PriorityQueue`和`Queue`的区别在于，它的出队顺序与元素的优先级有关，对`PriorityQueue`调用`remove()`或`poll()`方法，返回的总是优先级最高的元素。

要使用`PriorityQueue`，我们就必须给每个元素定义“优先级”。我们以实际代码为例，先看看`PriorityQueue`的行为：

```java
import java.util.PriorityQueue;
import java.util.Queue;

public class Main {
    public static void main(String[] args) {
        Queue<String> q = new PriorityQueue<>();
        // 添加3个元素到队列:
        q.offer("apple");
        q.offer("pear");
        q.offer("banana");
        System.out.println(q.poll()); // apple
        System.out.println(q.poll()); // banana
        System.out.println(q.poll()); // pear
        System.out.println(q.poll()); // null,因为队列为空
    }
}
```

我们放入的顺序是`"apple"`、`"pear"`、`"banana"`，但是取出的顺序却是`"apple"`、`"banana"`、`"pear"`，这是因为从字符串的排序看，`"apple"`排在最前面，`"pear"`排在最后面。

**因此，放入`PriorityQueue`的元素，必须实现`Comparable`接口，`PriorityQueue`会根据元素的排序顺序决定出队的优先级。**

如果我们要放入的元素并没有实现`Comparable`接口怎么办？`PriorityQueue`允许我们提供一个`Comparator`对象来判断两个元素的顺序。我们以银行排队业务为例，实现一个`PriorityQueue`：

```java
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.Queue;

public class Main {
    public static void main(String[] args) {
        Queue<User> q = new PriorityQueue<>(new UserComparator());
        // new UserComparator()也可以采用匿名类的实现方式
        // 添加3个元素到队列:
        q.offer(new User("Bob", "A1"));
        q.offer(new User("Alice", "A2"));
        q.offer(new User("Boss", "V1"));
        System.out.println(q.poll()); // Boss/V1
        System.out.println(q.poll()); // Bob/A1
        System.out.println(q.poll()); // Alice/A2
        System.out.println(q.poll()); // null,因为队列为空
    }
}

class UserComparator implements Comparator<User> {
    public int compare(User u1, User u2) {
        if (u1.number.charAt(0) == u2.number.charAt(0)) {
            // 如果两人的号都是A开头或者都是V开头,比较号的大小:
            return u1.number.compareTo(u2.number);
        }
        if (u1.number.charAt(0) == 'V') {
            // u1的号码是V开头,优先级高:
            return -1;
        } else {
            return 1;
        }
    }
}

class User {
    public final String name;
    public final String number;

    public User(String name, String number) {
        this.name = name;
        this.number = number;
    }

    public String toString() {
        return name + "/" + number;
    }
}
```

### Deque

我们知道，`Queue`是队列，只能一头进，另一头出。

如果把条件放松一下，允许两头都进，两头都出，这种队列叫双端队列（Double Ended Queue），学名`Deque`。

注意到`Deque`接口实际上扩展自`Queue`。

Java集合提供了接口`Deque`来实现一个双端队列，它的功能是：

- 既可以添加到队尾，也可以添加到队首；
- 既可以从队首获取，又可以从队尾获取。

我们来比较一下`Queue`和`Deque`出队和入队的方法：

|                    | Queue                  | Deque                           |
| :----------------- | :--------------------- | ------------------------------- |
| 添加元素到队尾     | add(E e) / offer(E e)  | addLast(E e) / offerLast(E e)   |
| 取队首元素并删除   | E remove() / E poll()  | E removeFirst() / E pollFirst() |
| 取队首元素但不删除 | E element() / E peek() | E getFirst() / E peekFirst()    |
| 添加元素到队首     | 无                     | addFirst(E e) / offerFirst(E e) |
| 取队尾元素并删除   | 无                     | E removeLast() / E pollLast()   |
| 取队尾元素但不删除 | 无                     | E getLast() / E peekLast()      |

`Deque`是一个接口，它的实现类有`ArrayDeque`和`LinkedList`。

我们发现`LinkedList`真是一个全能选手，它即是`List`，又是`Queue`，还是`Deque`。

**避免把`null`添加到队列。**

### Stack

在Java中，我们用`Deque`可以实现`Stack`的功能：

- 把元素压栈：`push(E)`/`addFirst(E)`；
- 把栈顶的元素“弹出”：`pop()`/`removeFirst()`；
- 取栈顶元素但不弹出：`peek()`/`peekFirst()`。

为什么Java的集合类没有单独的`Stack`接口呢？因为有个遗留类名字就叫`Stack`，出于兼容性考虑，所以没办法创建`Stack`接口，只能用`Deque`接口来“模拟”一个`Stack`了。







JavaSE阶段回顾总结：

https://www.bilibili.com/video/BV1MJ411v7tJ