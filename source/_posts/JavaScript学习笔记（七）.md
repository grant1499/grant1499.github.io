---
title: JavaScript学习笔记（七）
tags:
  - JavaScript
categories:
  - - JavaScript
mathjax: true
copyright: true
abbrlink: 4dcb7a67
date: 2021-08-30 16:44:59
---

## 1.更新DOM节点

<!--more-->

```js
// 获取<p id="p-id">...</p>
var p = document.getElementById('p-id');
// 设置文本为abc:
p.innerHTML = 'ABC'; // <p id="p-id">ABC</p>
// 设置HTML:
p.innerHTML = 'ABC <span style="color:red">RED</span> XYZ';
// <p>...</p>的内部结构已修改
p.innerText = '<script>alert("Hi")</script>';
// HTML被自动编码，无法设置一个<script>节点:
// <p id="p-id">&lt;script&gt;alert("Hi")&lt;/script&gt;</p>
```

1. innerText：修改文本的值，无法设置任何HTML标签
2. innerHTML：可以解析HTML文本标签

修改CSS也是经常需要的操作。DOM节点的`style`属性对应所有的CSS，可以直接获取或设置。

```js
// 获取<p id="p-id">...</p>
var p = document.getElementById('p-id');
// 设置CSS:
p.style.color = '#ff0000';
p.style.fontSize = '20px';
p.style.paddingTop = '2em';
```

## 2.删除DOM节点

要删除一个节点，首先要获得**该节点本身以及它的父节点**，然后，调用父节点的`removeChild`把自己删掉：

```html
<body>
    <div id="div1">
        <p id="p1">这是一个段落。</p>
        <p id="p2">这是另外一个段落。</p>
    </div>

    <script> // script标签必须放在body之中
        var child = document.getElementById("p1");
        var parent = document.getElementById("div1"); // 等价于var parent = p1.parentElement;
        parent.removeChild(child); // 等价于 parent.removeChild(p1);
    </script>
</body>
```

```js
<body>
    <dl id="app">
        <dt id="java">Java</dt>
        <dd id="javase">JavaSE</dd>
        <dd>JavaEE</dd>
    </dl>

    <script src="js/test.js"></script>
</body>
# ---js/test.js---
var dl = document.getElementById("app");
var dd = document.getElementsByTagName("dd");
dl.removeChild(dd[0]);
dl.removeChild(dd[0]);
```

**注意：**删除多个节点的时候，children是在时刻变化的，删除节点的时候一定要注意。

以上代码第13行删除“JavaSE”之后，”JavaEE“会**动态移动**`dd[0]`的位置！！

补充：**<script>标签在html中的位置会影响html页面和script脚本的加载顺序！**

## 3.创建和插入DOM节点

更新DOM节点中提到的`innerText`和`innerHTML`是对标签原文本的覆盖，也相当于插入节点。

如果标签原文本需要保留，就不能用上面的方法更新DOM节点。

示例：将JavaScript移动到list的末尾（me的后面）。

```js
<body>
    <p id="js">JavaScript</p>
        <div id="list">
            <p id="se">JavaSE</p>
            <p id="ee">JavaEE</p>
            <p id="me">JavaME</p>
        </div>

    <script src="js/test.js"></script>
</body>
# ---js/test.js---
var js = document.getElementById("js");
var list = document.getElementById("list");
list.appendChild(js); // 追加到后面，JavaScript被移动了

var newP = document.createElement("p"); // 创建一个p标签
newP.id = "newP";
newP.innerHTML = "hello,JavaScript";
list.append(newP);

//创建一个script标签节点
var myScript = document.creatElement('script');
myScript.setAttribute('type','text/javascript'); // 这种写法和myScript.type = xxx类似

//可以创建一个style标签
var myStyle = document.creatElement('style');//创建了一个空style标签
myStyle.setAttribute('type','text/css');
myStyle.innerHTML = 'body{background-color:chartreuse;}';//设置标签内容

document.getElementByTagName('head')[0].appendChild(myStyle);
```

**insertBefore：**

如果我们要把子节点插入到指定的位置怎么办？可以使用`parentElement.insertBefore(newElement, referenceElement);`，子节点会插入到`referenceElement`之前。

```js
var ee = document.getElementById('ee');
var js = document.getElementById('js');
var list = document.getElementById('list');
//要包含的节点.insertBefore(newNode,targetNode)
list.insertBefore(js,ee);
```

## 4.jQuery API学习网站

https://jquery.cuishifeng.cn/index.html。

## 5.操作表单

用JavaScript操作表单和操作DOM是类似的，因为表单本身也是DOM树。

表单的目的：提交信息，js操作表单来获得提交的信息。

HTML表单的输入控件主要有以下几种：

- 文本框，对应的`<input type="text">`，用于输入文本；
- 口令框，对应的`<input type="password">`，用于输入口令；
- 单选框，对应的`<input type="radio">`，用于选择一项；
- 复选框，对应的`<input type="checkbox">`，用于选择多项；
- 下拉框，对应的`<select>`，用于选择一项；
- 隐藏文本，对应的`<input type="hidden">`，用户不可见，但表单提交时会把隐藏文本发送到服务器。

### 获取值

如果我们获得了一个`<input>`节点的引用，就可以直接调用`value`获得对应的用户输入值：

```js
// <input type="text" id="email">
var input = document.getElementById('email');
input.value; // '用户输入的值'
```

这种方式可以应用于`text`、`password`、`hidden`以及`select`。但是，对于单选框和复选框，`value`属性返回的永远是HTML预设的值，而我们需要获得的实际是用户是否“勾上了”选项，所以应该用`checked`判断：

```js
// <label><input type="radio" name="weekday" id="monday" value="1"> Monday</label>
// <label><input type="radio" name="weekday" id="tuesday" value="2"> Tuesday</label>
var mon = document.getElementById('monday');
var tue = document.getElementById('tuesday');
mon.value; // '1'
tue.value; // '2'
mon.checked; // true或者false
tue.checked; // true或者false
```

### 设置值

设置值和获取值类似，对于`text`、`password`、`hidden`以及`select`，直接设置`value`就可以：

```js
// <input type="text" id="email">
var input = document.getElementById('email');
input.value = 'test@example.com'; // 文本框的内容已更新
```

对于单选框和复选框，设置`checked`为`true`或`false`即可。

### 提交表单

通过`<form>`元素的`submit()`方法提交一个表单，例如，响应一个`<button>`的`click`事件，提交表单：

```html
<!-- HTML -->// 表单绑定提交函数，onsubmit=绑定一个提交检测的函数
<form id="login-form" method="post" onsubmit="return checkForm()">
    <input type="text" id="username" name="username">
    <input type="password" id="password" name="password">
    <button type="submit">Submit</button>
</form>

<script>
function checkForm() {
    var pwd = document.getElementById('password');
    // 把用户输入的明文变为MD5:
    pwd.value = toMD5(pwd.value);
    // 继续下一步:
    return true;
}
</script>
```

注意要`return true`来告诉浏览器继续提交，如果`return false`，浏览器将不会继续提交form，这种情况通常对应用户输入有误，提示用户错误信息后终止提交form。

