---
title: JavaScript学习笔记（六）
tags:
  - JavaScript
categories:
  - - JavaScript
mathjax: true
copyright: true
abbrlink: 9fcb0df8
date: 2021-08-29 14:19:55
---

## 1.操作BOM对象（重点）

浏览器对象模型 (BOM) 使 JavaScript 有能力与浏览器"对话"。

浏览器对象模型（**B**rowser **O**bject **M**odel (BOM)）尚无正式标准。

<!--more-->

### Window 对象（重要）

所有浏览器都支持 window 对象。它表示浏览器窗口。

所有 JavaScript 全局对象、函数以及变量均自动成为 window 对象的成员。

全局变量是 window 对象的属性。

全局函数是 window 对象的方法。

Window尺寸：

- window.innerHeight - 浏览器窗口的内部高度(包括滚动条)
- window.innerWidth - 浏览器窗口的内部宽度(包括滚动条)
- 对于外部尺寸，有outerHeight和outerWidth

---

Navigator封装了浏览器的信息，不建议使用，因为可以人为修改！

### Window Screen

window.screen 对象包含有关用户屏幕的信息。可以省略前面的window。

`screen.height
`

`screen.width`

### Window Location（重要）

window.location 对象用于获得当前页面的地址 (URL)，并把浏览器重定向到新的页面。

![image-20210830091329489](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232126241.png)

### document（DOM）

`document`对象表示当前页面。由于HTML在浏览器中以DOM形式表示为树形结构，`document`对象就是整个DOM树的根节点。

`document`的`title`属性是从HTML文档中的`<title>xxx</title>`读取的，但是可以动态改变：

```js
document.title = '努力学习JavaScript!';
```

用`document`对象提供的`getElementById()`和`getElementsByTagName()`可以按ID获得一个DOM节点和按Tag名称获得一组DOM节点：

```html
<dl id="app">
    <dt>Java</dt>
	<dd>JavaSE</dd>
	<dd>JavaEE</dd>
</dl>
var dl = document.getElementById("app");
var dd = document.getElementsByTagName("dd");
```

`document`对象还有一个`cookie`属性，可以获取当前页面的Cookie。

Cookie是由服务器发送的key-value标示符。因为HTTP协议是无状态的，但是服务器要区分到底是哪个用户发过来的请求，就可以用Cookie来区分。当一个用户成功登录后，服务器发送一个Cookie给浏览器，例如`user=ABC123XYZ(加密的字符串)...`，此后，浏览器访问该网站时，会在请求头附上这个Cookie，服务器根据Cookie即可区分出用户。

Cookie还可以存储网站的一些设置，例如，页面显示的语言等等。

JavaScript可以通过`document.cookie`读取到当前页面的Cookie：

```js
document.cookie
"Hm_lvt_2efddd14a5f2b304677462d06fb4f964=1630192613,1630227410,1630241840,1630284920; Hm_lpvt_2efddd14a5f2b304677462d06fb4f964=1630284928"
```

由于JavaScript能读取到页面的Cookie，而用户的登录信息通常也存在Cookie中，这就造成了巨大的安全隐患。

```html
<!-- 当前页面在wwwexample.com -->
<html>
    <head>// 劫持cookie
        <script src="http://www.foo.com/jquery.js"></script>
    </head>
    ...
</html>
```

如果引入的第三方的JavaScript中存在恶意代码，则`www.foo.com`网站将直接获取到`www.example.com`网站的用户登录信息。

为了确保安全，服务器端在设置Cookie时，应该始终坚持使用`httpOnly`。这由浏览器实现。

history代表浏览器的历史记录，不建议使用。

## 2.操作DOM对象（重点）

文档对象模型（Document Object Model）

由于HTML文档被浏览器解析后就是一棵DOM树，要改变HTML的结构，就需要通过JavaScript来操作DOM。

始终记住DOM是一个树形结构。操作一个DOM节点实际上就是这么几个操作：

- 更新：更新该DOM节点的内容，相当于更新了该DOM节点表示的HTML的内容；
- 遍历：遍历该DOM节点下的子节点，以便进行进一步操作；
- 添加：在该DOM节点下新增一个子节点，相当于动态增加了一个HTML节点；
- 删除：将该节点从HTML中删除，相当于删掉了该DOM节点的内容以及它包含的所有子节点。

**在操作一个DOM节点前，我们需要通过各种方式先拿到这个DOM节点。**

**注意！！！**操作DOM节点时script标签应该放在`<body>`内部，不要放在`<head>`内部。**否则拿不到节点。**

由于ID在HTML文档中是唯一的，所以`document.getElementById()`可以直接定位唯一的一个DOM节点。`document.getElementsByTagName()`和`document.getElementsByClassName()`总是返回一组DOM节点。要精确地选择DOM，可以先定位父节点，再从父节点开始选择，以缩小范围。

```js
// 对应CSS的各种选择器
// 返回ID为'test'的节点：
var test = document.getElementById('test');

// 先定位ID为'test-table'的节点，再返回其内部所有tr节点：
var trs = document.getElementById('test-table').getElementsByTagName('tr');

// 先定位ID为'test-div'的节点，再返回其内部所有class包含red的节点：
var reds = document.getElementById('test-div').getElementsByClassName('red');

// 获取节点test下的所有直属子节点:
var cs = test.children;

// 获取节点test下第一个、最后一个子节点：
var first = test.firstElementChild;
var last = test.lastElementChild;
```

上面是原生代码，之后我们尽量都使用`jQuery();`