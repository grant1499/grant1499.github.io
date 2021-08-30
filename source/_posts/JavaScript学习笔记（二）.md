---
title: JavaScript学习笔记（二）
tags:
  - JavaScript
categories:
  - - JavaScript
mathjax: true
copyright: true
abbrlink: 6f683951
date: 2021-08-28 12:04:06
---

## 1.数组详解

下面的代码创建名为 cars 的数组：

<!--more-->

和Java一样，JS数组下标也是从0开始。

```js
var cars=new Array();
cars[0]="Saab";
cars[1]="Volvo";
cars[2]="BMW";
```

或者 (condensed array)：

```js
var cars=new Array("Saab","Volvo","BMW");
```

或者 (literal array)：尽量采取这种写法

```js
var cars=["Saab","Volvo","BMW"];
```

和python列表类似，JS数组元素可以是不同类型。

数组下标越界不会报错，会出现undefined。

**访问与修改数组：**

```js
var name=myCars[0];
myCars[0]="Opel";
```

**数组长度**：arr.length，可以修改加长，多余的元素会是undefined，如果赋值过小，元素就会丢失。

**JS中的数组非常灵活，是可变长的。**

```js
let arr = [1,2,3,"1",2.3]
// indexOf方法，通过元素获得下标索引，字符串的"1"和数字 1 是不同的
console.log(arr.indexOf(1)) // 0
console.log(arr.indexOf("1")) // 3
```

**裁剪数组**：slice方法，类似python中的列表切片

截取数组得到一部分，返回一个新数组

它有两种写法，一个或者两个参数，从开始截取到末尾，顾前不顾后，省略参数截取到末尾。

---

在处理数组时，删除元素和添加新元素是很简单的。

可以用push和pop方法。**（都是尾部操作）**

pop() 方法从数组中删除最后一个元素，并返回被删除的元素。

push() 方法（在数组结尾处）向数组添加一个或多个元素新的元素，并返回新数组的长度。

还可以用shift()和unshift()方法。**（都是头部操作）**

shift() 方法会删除首个数组元素，并把所有其他元素“位移”到更低的索引，返回删除元素。

unshift() 方法（在开头）向数组添加一个或多个新元素，并“反向位移”旧元素，返回新数组的长度。

---

**数组排序：**

sort() 方法以字母顺序对数组进行排序。

**反转数组：**

reverse() 方法反转数组中的元素。

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort();            // 对 fruits 中的元素进行排序
fruits.reverse();         // 反转元素顺序
```

**合并（拼接）数组：**和字符串一样拼接

concat() 方法通过合并（连接）现有数组来创建一个新数组：

concat() 方法不会更改现有数组。它总是返回一个新数组。

可以使用任意数量的数组参数，也可以将数组值作为参数。

```js
var arr1 = ["Cecilie", "Lone"];
var arr2 = ["Emil", "Tobias", "Linus"];
var arr3 = ["Robin", "Morgan"];
var myChildren = arr1.concat(arr2, arr3);   // 将arr1、arr2 与 arr3 连接在一起
// 
var arr1 = ["Cecilie", "Lone"];
var myChildren = arr1.concat(["Emil", "Tobias", "Linus"]); 
```

join() 方法也可将所有数组元素结合为一个字符串，还可以规定分隔符。

```js
var fruits = ["Banana", "Orange","Apple", "Mango"];
document.getElementById("demo").innerHTML = fruits.join(" * "); 
// Banana * Orange * Apple * Mango，是一个字符串
```

**多维数组：**

```js
let arr = [[1,2],[3,4],["5","6"]]
// arr[1][1] = 4
```

常用的数组方法小结：

slice()截断；push()，pop()，shift()，unshift()插入删除元素；concat()拼接数组。

## 2.对象

对象由花括号分隔。在括号内部，对象的属性以名称和值对的形式 (name : value) 来定义。属性由逗号分隔：

**对象也就是若干个键值对。**

在 JavaScript中，几乎所有的事物都是对象。

注意：JS中，所有的键都是字符串，值可以是任意对象。

```js
var person={
firstname : "John",
lastname  : "Doe",
id        :  5566,
tags : ["C++","Java","JavaScript"]
};
```

对象属性有两种寻址方式：

```js
name=person.lastname;// 类似Java访问对象属性
name=person["lastname"];// 类似python字典访问方式
// 给对象属性赋值修改也是一样的方法
```

使用一个不存在的对象属性不会报错，会出现undefined。

动态删除对象属性：`delete person.id`，会返回true。

动态添加对象属性：直接给新属性添加值就行，`person.score = 75`。

对象成员归属检查：通过in运算符判断。

```js
var o = {  //定义对象
    a : 1,  //定义属性a
    b : function() {}  //定义方法b
}
console.log("a" in o);  //返回true
console.log("b" in o);  //返回true
console.log("c" in o);  //返回false
console.log("valueOf" in o);  //返回true，继承Object的原型方法
console.log("constructor" in o);  //返回true，继承Object的原型属性
```

判断一个属性是否是这个对象自身拥有的（继承的属性除外） ，hasOwnProperty()方法。

```js
var o = {  //定义对象
    a : 1,  //定义属性a
    b : function() {}  //定义方法b
}
console.log(o.hasOwnProperty("a"));  //返回true
console.log(o.hasOwnProperty("c"));  //返回false
console.log(o.hasOwnProperty("valueOf")); //返回false
```

## 3.严格检查模式strict

`i = 1`，对于这句代码，JS不会报错，会认为i是一个全局变量，容易冲突，应谨慎使用。

使用严格检查模式，确保：IDEA或者WebStorm设置支持ES6语法。

`'use strict';`严格检查模式，必须写在JavaScript的第一行！

对于Vscode，可以用语法检查插件。

var类似成员变量，let类似局部变量，建议使用let。

## 4.字符串详解

JS并不区分字符和字符串。

JS中字符串索引，转义字符都与Java相同。

多行字符串：

```js
//tab 上面 esc下面
var msg =
    `hello
world
你好呀
nihao
`
```

模板字符串：

```js
let Name = "wangyangming";
let age = 21;
console.log(`hello,${Name}!`);// 注意模板字符串必须使用`，不能用引号
```

JS中的字符串索引同Java类似，不能使用[]提取，需要使用charAt()函数来提取。

**同Java中的String类似，JS字符串也是不可变的！**

| 方法          | 描述                                                   |
| ------------- | ------------------------------------------------------ |
| charAt()      | 返回指定索引位置的字符                                 |
| charCodeAt()  | 返回指定索引位置字符的 Unicode 值                      |
| concat()      | 连接两个或多个字符串，返回连接后的字符串               |
| substring()   | 提取字符串中两个指定的索引号之间的字符，**顾前不顾后** |
| indexOf()     | 返回字符串中检索指定字符第一次出现的位置               |
| lastIndexOf() | 返回字符串中检索指定字符最后一次出现的位置             |
| toLowerCase() | 把字符串转换为小写                                     |
| toUpperCase() | 把字符串转换为大写                                     |

这些方法和Java中的方法非常像，就当复习一遍Java。