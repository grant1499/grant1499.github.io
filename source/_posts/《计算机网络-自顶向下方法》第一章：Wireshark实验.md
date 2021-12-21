---
title: 《计算机网络--自顶向下方法》第一章：Wireshark实验
tags:
  - 计算机网络
categories:
  - - 计算机网络
mathjax: false
copyright: true
abbrlink: 8e8a16da
date: 2021-09-19 14:36:33
---

# 第一章：Wireshark实验

《计算机网络--自顶向下方法》[笔记及配套资料](https://github.com/moranzcw/Computer-Networking-A-Top-Down-Approach-NOTES)。

《计算机网络--自顶向下方法》资料： https://github.com/jzplp/Computer-Network-A-Top-Down-Approach-Answer。

实验与习题答案： https://blog.csdn.net/qq278672818/category_8368262.html。

**在线实验平台： https://www.educoder.net/paths/734。（强烈推荐！）**

<!--more-->

本实验地址： https://github.com/moranzcw/Computer-Networking-A-Top-Down-Approach-NOTES/blob/master/WiresharkLab/Wireshark%E5%AE%9E%E9%AA%8C-Intro/Wireshark%E5%AE%9E%E9%AA%8C-Intro.md。

本次实验参考教程： https://www.cnblogs.com/linfangnan/p/12674066.html。

Wireshark是一个免费的分组嗅探器。

在第一个Wireshark实验中，你会熟悉Wireshark，并做一些简单的抓包和观察。

软件下载安装直接去[官网](http://www.wireshark.org/)下载即可。

下载安装使用入门教程： http://c.biancheng.net/view/6379.html。（中间会安装一个别的软件）

Wireshark抓包详细教程： https://www.cnblogs.com/mq0036/p/11187138.html。

在Windows的开始界面找到Wireshark，点击进入。

选择WLAN开始捕获，在浏览器中输入示例网站。

示例网站： http://gaia.cs.umass.edu/wireshark-labs/INTRO-wireshark-file1.html。

在主 Wireshark 窗口顶部的分组显示过滤器窗口中，键入 “http”（不含引号，且小写 - Wireshark中的所有协议名称均为小写）。然后就只会看到HTTP消息。

![image-20210919150722493](https://gitee.com/grant1499/blog-pic/raw/master/img/202110231742462.png)

找到从您的计算机发送到 gaia.cs.umass.edu HTTP 服务器的 HTTP GET 消息。（在 Wireshark 分组列表窗口中查找 HTTP GET 消息，其后的 gaia.cs.umass.edu URL 中显示 “GET”）。

也就是上图中颜色更深的消息。双击它可以看到具体信息。

本次实验就到这里。

---

需要提交的内容：

第一个实验的目的主要是向你介绍Wireshark。以下问题将证明您已经能够使Wireshark启动并运行，并且已经探索了其中一些功能。根据您的Wireshark实验回答以下问题：

1. 列出上述步骤7中出现在未过滤的分组列表窗口的协议列中的3种不同的协议。
2. 从HTTP GET消息发送到HTTP OK回复需要多长时间？ (默认情况下，分组列表窗口中的时间列的值是自Wireshark开始捕获以来的时间(以秒为单位)。要想以日期格式显示时间，请选择Wireshark的“视图”下拉菜单，然后选择“时间显示格式”，然后选择“日期和时间”。)
3. gaia.cs.umass.edu(也称为wwwnet.cs.umass.edu)的Internet地址是什么？您的计算机的Internet地址是什么？
4. 打印问题2提到的两个HTTP消息(GET和OK)。要这样做，从Wireshark的“文件”菜单中选择“打印”，然后选择“仅选中分组”和“按当前显示”按钮，然后单击确定。

回答：

1.分别是:OICQ、HTTP、TCP。做实验时有其他网页干扰，排除后应该只有以上 3 种。

2.选择http过滤消息，然后视图-->时间显示格式-->日期和时间，

![image-20210919151905054](https://gitee.com/grant1499/blog-pic/raw/master/img/202110231742482.png)

计算一下时间差：

![image-20210919152011247](https://gitee.com/grant1499/blog-pic/raw/master/img/202110231742498.png)

3.观察上面http过滤的图，发现：

gaia.cs.umass.edu IP: 128.119.245.12

我的 IP: 192.168.195.79

4.按题目要求打印就行。