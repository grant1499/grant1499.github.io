---
title: JavaScript学习笔记（五）
tags:
  - JavaScript
categories:
  - - JavaScript
mathjax: true
copyright: true
abbrlink: fac54621
date: 2021-08-29 10:50:48
---

### 8.3 JSON

JSON是JavaScript Object Notation的缩写，它是一种数据交换格式。

JSON 通常用于服务端向网页传递数据 。

<!--more-->

JSON 使用 JavaScript 语法，但是 JSON 格式仅仅是一个文本。

在JavaScript中一切皆是对象，任何JS支持的类型都可以用JSON来表示。

在JSON中，一共就这么几种数据类型：

- number：和JavaScript的`number`完全一致；
- boolean：就是JavaScript的`true`或`false`；
- string：就是JavaScript的`string`；
- null：就是JavaScript的`null`；
- array：就是JavaScript的`Array`表示方式——`[]`；
- object：就是JavaScript的`{ ... }`表示方式。
- 所有键值对都使用`key:value`

以及上面的任意组合。

json字符串与JS对象的相互转换：序列化和反序列化

```js
var user = {
    name : "tom",
    age : 18,
    id : 2001
}

// 1.JS对象--》JSON字符串
var jsonUser = JSON.stringify(user);
console.log(jsonUser)
// {"name":"tom","age":18,"id":2001}

// 2.JSON字符串--》JS对象
var obj = JSON.parse('{"name":"tom","age":18,"id":2001}') // 注意单、双引号
obj
// {name: "tom", age: 18, id: 2001}
```

## 9.面向对象-->原型继承

JavaScript的面向对象编程和大多数其他语言如Java、C#的面向对象编程都不太一样。如果你熟悉Java或C#，很好，你一定明白面向对象的两个基本概念：

- 类：类是对象的类型模板；

- 实例：实例是根据类创建的对象。

JavaScript不区分类和实例的概念，而是通过**原型（prototype）**来实现面向对象编程。

当我们用`obj.xxx`访问一个对象的属性时，JavaScript引擎先在当前对象上查找该属性，如果没有找到，就到其原型对象上找，如果还没有找到，就一直上溯到`Object.prototype`对象，最后，如果还没有找到，就只能返回`undefined`。

```js
var Student = {
    name : "wang",
    age : 19,
    run : function(){
        alert(this.name + " is running...");
    }
};

var xiaoming = {
    name : "xiaoming"
};

xiaoming.__proto__ = Student;// 小明得到原型指向了Student
xiaoming.run()// 小明继承了Student的方法
```

JavaScript的原型链和Java的Class区别就在，它没有“Class”的概念，所有对象都是实例，所谓继承关系不过是把一个对象的原型指向另一个对象而已。

如果你把`xiaoming`的原型再指向其他对象：

```js
var Bird = {
    fly: function () {
        alert(this.name + ' is flying...');
    }
};

xiaoming.__proto__ = Bird;
// xiaoming.run()，此时小明跑不了了
xiaoming.fly()
```

## 10.构造函数

除了直接用`{ ... }`创建一个对象外，JavaScript还可以用一种构造函数的方法来创建对象。它的用法是，先定义一个构造函数：

```js
function Student(name) {
    this.name = name;
    this.hello = function () {
        alert('Hello, ' + this.name + '!');
    }
}
```

你会问，咦，这不是一个普通函数吗？

这确实是一个普通函数，但是在JavaScript中，可以用关键字`new`来调用这个函数，并返回一个对象：

```js
var xiaoming = new Student('小明');
xiaoming.name; // '小明'
xiaoming.hello(); // Hello, 小明!
```

*注意*，如果不写`new`，这就是一个普通函数，它返回`undefined`。但是，如果写了`new`，它就变成了一个构造函数，它绑定的`this`指向新创建的对象，并默认返回`this`，也就是说，不需要在最后写`return this;`。

新创建的`xiaoming`的原型链是：

```ja
xiaoming ----> Student.prototype ----> Object.prototype ----> null
```

```js
// 给Student原型增加一个方法
function Student(name) {
    this.name = name;
}

Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
};
```

## 11.class继承

JavaScript的对象模型是基于原型实现的，特点是简单，缺点是理解起来比传统的类－实例模型要困难，最大的缺点是继承的实现需要编写大量代码，并且需要正确实现原型链。

新的关键字`class`从ES6开始正式被引入到JavaScript中。`class`的目的就是让定义类更简单。

定义一个类：像Java一样

```js
class Student {
    constructor(name) {// 构造方法
        this.name = name;
    }

    hello() {// 不需要加上function关键字
        alert('Hello, ' + this.name + '!');
    }
}
var xiaoming = new Student('小明');
xiaoming.hello();
```

class继承：这很Java！

```js
class Pupil extends Student{
    constuctor(name,grader){
        super(name);// 记得用super调用父类的构造方法!
        this.grade = grade;
    }
}
```

用`class`的好处就是极大地简化了原型链代码。原型继承和class继承没有本质区别。

现在用还早了点，因为不是所有的主流浏览器都支持ES6的class。如果一定要现在就用上，就需要一个工具把`class`代码转换为传统的`prototype`代码，可以试试[Babel](https://babeljs.io/)这个工具。

原型链参考文章： https://www.jianshu.com/p/be7c95714586。

![image-20210829141743641](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232127222.png)