---
title: JavaScript学习笔记（四）
abbrlink: 955b8366
date: 2021-08-28 16:11:37
tags:
  - JavaScript
categories:
  - - JavaScript
mathjax: true
copyright: true
---

## 1.变量提升

JavaScript的函数定义有个特点，它会先扫描整个函数体的语句，把所有申明的变量“提升”到函数顶部：

<!--more-->

```js
'use strict';

function foo() {
    var x = 'Hello, ' + y;// 输出Hello, undefined
    console.log(x);
    var y = 'Bob';
}

foo();
```

对于上述`foo()`函数，JavaScript引擎看到的代码相当于：

```js
function foo() {
    var y; // 提升变量y的申明，此时y为undefined
    var x = 'Hello, ' + y;
    console.log(x);
    y = 'Bob';
}
```

养成规范：所有 的变量定义都放在函数的头部，不要乱放，便于代码维护。

```js
function foo() {
    var
        x = 1, // x初始化为1
        y = x + 1, // y初始化为2
        z, i; // z和i为undefined
    // 其他语句:
    for (i=0; i<100; i++) {
        ...
    }
}
```

## 2.全局变量

变量在函数外定义，即为全局变量。

全局变量有 **全局作用域**: 网页中所有脚本和函数均可使用。 

```js
var x = 2;// 全局变量
function f(){
    console.log(x);// 2
}
f()
```

变量生命周期：

JavaScript 变量生命周期在它声明时初始化。

局部变量在函数执行完毕后销毁。

全局变量在页面关闭后销毁。

**全局对象window**

在 HTML 中, 全局变量是 window 对象: 所有数据变量都属于 window 对象。

```js
'use strict';

var course = 'Learn JavaScript';
alert(course); // 'Learn JavaScript'
alert(window.course); // 'Learn JavaScript'
// 效果一样
```

alert() 这个函数本身也是一个window的变量。

JavaScript实际上只有一个全局作用域。任何变量（函数也视为变量），如果没有在当前函数作用域中找到，就会继续往上查找，最后如果在全局作用域中也没有找到，则报`ReferenceError`错误。

## 3.名字空间

全局变量会绑定到`window`上，不同的JavaScript文件如果使用了相同的全局变量，或者定义了相同名字的顶层函数，都会造成命名冲突，并且很难被发现。

如何解决冲突？

把自己的所有变量和函数全部绑定到一个全局变量中。

```js
// 唯一的全局变量MYAPP:也就是一个空对象
var MYAPP = {};

// 其他变量:
MYAPP.name = 'myapp';
MYAPP.version = 1.0;

// 其他函数:
MYAPP.foo = function () {
    return 'foo';
};
```

也就引入了名字空间，类似C++中的namespace。

## 4.局部作用域

由于JavaScript的`var`关键字申明的变量作用域实际上是函数内部，我们在`for`循环等语句块中是无法定义具有局部作用域的变量的。

所以ES6引入了新的关键字`let`，用`let`替代`var`可以申明一个块级作用域的变量。

块级作用域也就是一个``{}``的范围。

建议：只在开一个全局变量的名字空间时使用`var`，其他情况使用`let`关键字。

## 5.常量

ES6标准引入了新的关键字`const`来定义常量，`const`与`let`都具有块级作用域：

```js
'use strict';
const PI = 3.14;
PI = 3; // 某些浏览器不报错，但是无效果！
PI; // 3.14
```

## 6.方法

在一个对象中绑定函数，称为这个对象的方法。

```js
var MYAPP = {};

MYAPP.student = {
    name : "tom",
    birth : 2001,
    age : function(){ // 方法
        let now = new Date().getFullYear();// 今年 - 出生年份
        return now - this.birth; // this指向当前对象
    }
};

alert(MYAPP.student.name);
alert(MYAPP.student.age());
```

`this`关键字的作用？

```js
var MYAPP = {};

function getAge(){
    let now = new Date().getFullYear();
    return now - this.birth;
}
MYAPP.student = {
    name : "tom",
    birth : 2001,
    age : getAge
};

alert(MYAPP.student.name);
//? 方式1，利用对象调用方法，this指代调用方法的对象student
alert(MYAPP.student.age());//? 正常显示20
//? 方式2，直接调用方法，this指代调用方法的对象window
alert(getAge());//? 显示NaN
```

这个`this`的指向是JS中的一大坑！！！

更坑爹的是，如果这么写：

```js
var fn = xiaoming.age; // 先拿到xiaoming的age函数
fn(); // NaN
```

也是不行的！要保证`this`指向正确，必须用`obj.xxx()`的形式调用！

ECMA决定，在strict模式下让函数的`this`指向`undefined`，因此，在strict模式下，你会得到一个错误。

## 7.控制this的指向

在JS中，我们可以控制this的指向。

要指定函数的`this`指向哪个对象，可以用函数本身的`apply`方法，它接收两个参数，第一个参数就是需要绑定的`this`变量，第二个参数是`Array`，表示函数本身的参数。

用`apply`修复`getAge()`调用：

```js
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this指向xiaoming, getAge参数为空
```

## 8.常用内部对象

### 8.1 标准对象

我们用`typeof`操作符获取对象的类型，它总是返回一个字符串：

```js
typeof 123; // 'number'
typeof NaN; // 'number'
typeof 'str'; // 'string'
typeof true; // 'boolean'
typeof undefined; // 'undefined'
typeof Math.abs; // 'function'
typeof null; // 'object'
typeof []; // 'object'
typeof {}; // 'object'
```

特别注意`null`的类型是`object`，`Array`的类型也是`object`，如果我们用`typeof`将无法区分出`null`、`Array`和通常意义上的object——`{}`。

### 8.2 Date

```js
var now = new Date();
now; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
now.getFullYear(); // 2015, 年份
now.getMonth(); // 5, 月份，注意月份范围是0~11，5表示六月
now.getDate(); // 24, 表示24号
now.getDay(); // 3, 表示星期三
now.getHours(); // 19, 24小时制
now.getMinutes(); // 49, 分钟
now.getSeconds(); // 22, 秒
now.getMilliseconds(); // 875, 毫秒数
now.getTime(); // 1435146562875, 以number形式表示的时间戳
```

注意，当前时间是浏览器从本机操作系统获取的时间，所以不一定准确，因为用户可以把当前时间设定为任何值。

你可能观察到了一个**非常非常坑爹**的地方，就是JavaScript的月份范围用整数表示是0~11，`0`表示一月，`1`表示二月……，所以要表示6月，我们传入的是`5`！

时间戳还原成标准时间：

```js
var t = new Date();
t.getTime()
// 1630205159435
var x = new Date(1630205159435);
x
// Sun Aug 29 2021 10:45:59 GMT+0800 (中国标准时间)
// 转化为时间字符串
var d = new Date(1435146562875);
d.toLocaleString(); // '2015/6/24 下午7:49:22'，本地时间（北京时区+8:00），显示的字符串与操作系统设定的格式有关
d.toUTCString(); // 'Wed, 24 Jun 2015 11:49:22 GMT'，UTC时间，与本地时间相差8小时
```

