---
title: JavaScript学习笔记（三）
tags:
  - JavaScript
categories:
  - - JavaScript
mathjax: true
copyright: true
abbrlink: 22769a03
date: 2021-08-28 16:11:29
---

## 1.流程控制

<!--more-->

if-else判断：

```js
let age = 7;

if (age > 6){
    alert("haha");
}
else if (age < 5){
    alert("hi");
}
else{
    alert("nihao");
}
```

while与do-while循环，以及for循环：

```js
var i = 0;
while (i < 10){
    if (i === 6) {break;}
    alert("i:" + i);
    i ++;
}
// do-while
do{
    if (i === 6) {break;}
    alert("i:" + i);
    i ++;
} while (i < 10);
// break和continue同java中类似
for (i = 0; i < 10; i++) {
    if (i === 3) { continue; }
    text += "数字是 " + i + "<br>";
} 
```

数组forEach循环：ES5.1新特性

```js
var arr = [1,24,4,5.6,3];
arr.forEach(function(value){
    console.log(value);
})
```

for...in 语句：用于遍历数组或者对象的属性。**遍历数组下标！**

```js
var arr = [1,24,4,5.6,3];
for (let i in arr){
    if (!arr.hasOwnProperty(i)){// 避免一些bug
        continue;
    }
    console.log(arr[i]);// 注意：i获取的是数组中元素的下标
}
```

参考文章： https://blog.csdn.net/wangpenglonton/article/details/79095011。

for-in遍历数组可能出现一些**bug**，，有两种方式避免

1.在循环数组集合时，不使用for-in，统一使用for(var i=0;i<length;i++)这种形式；

2.在for-in循环中增加一个hasOwnProperty的判断。（麻烦）

## 2.Map和Set

ES6的新特性~

JS中的Map对应Java中的HashMap，python中的字典，由键值对构成。

Map：映射

```js
let map = new Map([["tom",90],["jerry",76]])
let name = map.get("jerry") // 通过key获得value
console.log(name) // 76
map.set("admin",88) // 新增或修改
// Map(3) {"tom" => 90, "jerry" => 76, "admin" => 88}
map.set("tom",68)
// Map(3) {"tom" => 68, "jerry" => 76, "admin" => 88}
map.delete("tom") // 删除
// true
map
Map(2) {"jerry" => 76, "admin" => 88}
```

Set：无序不重复集合，会自动去重

```js
let set = new Set([1,2,3,2,1])
// Set(3) {1, 2, 3}
set.add(5) // 新加元素，会去重
// Set(4) {1, 2, 3, 5}
set.add(1)
// Set(4) {1, 2, 3, 5}
set.delete(2) // 删除元素
// true
set
// Set(3) {1, 3, 5}
set.has(5) // 判断是否含有
// true
```

## 3.for-of遍历

ES6新特性~

for-of可以遍历数组，Map，Set

```js
let set = new Set([1,2,3,2,1])
for (let i of set){
    console.log(i) // 遍历数组，Map也是一样的
}
```

建议数组以后就用for-of遍历，不要使用for-in。

## 4.函数

JS中的函数与Java大同小异。

```js
// 定义函数
function abs(x){
	if (x >= 0){
		return x;
	}
	else{
		return -x;
	}
}
// JS支持三目运算符
function ABS(x){
	return (x >= 0) ? x : -x;
} 
```

一旦执行到return代表函数结束，返回结果！

如果没有执行return，函数执行完也会返回结果，结果就是undefined。

和python一样，JS也有匿名函数，是第二种定义函数的方式。

```js
var abs = function (x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
};
```

上述两种定义*完全等价*，注意第二种方式按照完整语法需要在函数体末尾加一个`;`，表示赋值语句结束。

**参数问题：**

由于JavaScript允许传入任意个参数而不影响调用，因此传入的参数比定义的参数多也没有问题，虽然函数内部并不需要这些参数：

```js
abs(10, 'blablabla'); // 返回10
abs(-9, 'haha', 'hehe', null); // 返回9
```

传入的参数比定义的少也没有问题：

```js
abs(); // 返回NaN
```

此时`abs(x)`函数的参数`x`将收到`undefined`，计算结果为`NaN`。

要避免收到`undefined`，可以对参数进行检查：

```js
function abs(x) {
    if (typeof x !== 'number') {
        throw 'Not a number';// 手动抛出异常
    }
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```

**arguments：**

JavaScript还有一个免费赠送的关键字`arguments`，它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数。

```js
function foo(x) {
    console.log('x = ' + x); // 10
    for (var i=0; i<arguments.length; i++) {
        console.log('arg ' + i + ' = ' + arguments[i]); // 10, 20, 30
    }
}
foo(10, 20, 30);
```

实际上`arguments`最常用于判断传入参数的个数。

---

问题：arguments包含所有的参数，我们有时候想使用多余的参数来进行附加操作。需要排除已有参数~

**rest：**ES6引入的新特性，获取除了已经定义的参数之外的所有参数~

```js
function foo(a, b, ...rest) {
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}

foo(1, 2, 3, 4, 5);
// 结果:
// a = 1
// b = 2
// Array [ 3, 4, 5 ]

foo(1);
// 结果:
// a = 1
// b = undefined
// Array []
```

rest参数只能写在最后，前面用`...`标识，从运行结果可知，传入的参数先绑定`a`、`b`，多余的参数以数组形式交给变量`rest`，所以，不再需要`arguments`我们就获取了全部参数。

如果传入的参数连正常定义的参数都没填满，也不要紧，rest参数会接收一个空数组（注意不是`undefined`）。

## 5.变量的作用域

var申明变量的作用域：

如果一个变量在函数体内部申明，则该变量的作用域为整个函数体，在函数体外不可引用该变量。

```js
'use strict';
function foo() {
    var x = 1;
    x = x + 1;
}

x = x + 2; // ReferenceError! 无法在函数体外引用变量x
```

不同函数内部的同名变量互相独立，互不影响。

内部函数可以访问外部函数的成员，反之则不行。

```js
'use strict';

function foo() {
    var x = 1;
    function bar() {
        var y = x + 1; // bar可以访问foo的变量x!
    }
    var z = y + 1; // ReferenceError! foo不可以访问bar的变量y!
}
```

假设，内部函数变量和外部函数变量，重名！

函数查找变量从自身函数开始~， 由“内”向“外”查找，假设外部存在这个同名的函数变量，则内部函数会屏蔽外部函数的变量。

```js
function foo() {
    var x = 1;
    function bar() {
        var x = 'A';
        console.log('x in bar() = ' + x); // 'A'
    }
	bar();
    console.log('x in foo() = ' + x); // 1 
}
// 内、外函数中的x互不影响
foo();
```

