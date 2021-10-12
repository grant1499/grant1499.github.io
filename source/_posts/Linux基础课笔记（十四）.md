---
title: Linux基础课笔记（十四）
date: 2021-10-10 11:12:27
tags:
  - Linux
categories:
  - Linux
mathjax: false
copyright: true
---

## 1.5：Linux常用命令

参考文章： https://www.linuxprobe.com/basic-learning-02.html。

Linux命令非常多，本节讲解几个常用命令。其他命令依赖于大家根据实际操作环境，边用边查。

<!--more-->

本文贵在精不在全。

### 系统状况

1. top：查看所有进程的信息（Linux的任务管理器）
    - 打开后，输入M：按使用内存排序
    - 打开后，输入P：按使用CPU排序
    - 打开后，输入q：退出
2. df -h：查看硬盘使用情况
3. free -h：查看内存使用情况
4. du -sh：查看当前目录占用的硬盘空间
5. ps aux：查看所有进程
6. kill -9 pid：杀死编号为pid的进程
    - 传递某个具体的信号：kill -s SIGTERM pid，有些进程只能传递信号杀掉，不能用kill -9
7. netstat -nt：查看所有网络连接
8. w：列出当前登陆的用户
9. ping www.baidu.com：检查是否连网

### 文件权限

1. chmod：修改文件权限
    - chmod +x xxx：给xxx添加可执行权限；
    - chmod -x xxx：去掉xxx的可执行权限；
    - chmod 777 xxx：将xxx的权限改成777。
    
    linux系统内有档案有三种身份 u:拥有者 g:群组 o:其他人
    
    这些身份对于文档常用的有下面权限：
    
2. r：**读**权限，用户可以读取文档的内容，如用cat，more查看

    w：**写**权限，用户可以编辑文档

    x：该目录具有可以被系统**执行**的权限

    ```shell
    acs@c508de167641:~$ ls -l
    total 12
    drwxrwxr-x 8 acs acs 4096 Oct  6 09:10 homework # d之后三位字符代表一组权限
    drwxrwxr-x 3 acs acs 4096 Sep 17 22:56 project # 分别对应自己，群组，其他人
    drwxrwxr-x 6 acs acs 4096 Oct  1 09:00 thrift_lesson # 字母表示有权限，-表示没权限
    ```

### 文件检索

1. `find /path/to/directory/ -name '*.py'`：搜索某个文件路径下的所有*.py文件
2. `grep xxx`：从stdin中读入若干行数据，如果某行中包含xxx，则输出该行；否则忽略该行。
3. `wc`：统计行数、单词数、字节数
    - 既可以从stdin中直接读入内容；也可以在命令行参数中传入文件名列表；
    - `wc -l`：统计行数
    - `wc -w`：统计单词数
    - `wc -c`：统计字节数
4. `tree`：展示当前目录的文件结构
    - `tree /path/to/directory/`：展示某个目录的文件结构
    - `tree -a`：展示隐藏文件
5. `ag xxx`：搜索当前目录下的所有文件，检索xxx字符串，**全文搜索，重要功能！**
6. `cut`：分割一行内容
    - 从stdin中读入多行数据
    - `echo $PATH | cut -d ':' -f 3,5`：输出PATH用:分割后第3、5列数据
    - `echo $PATH | cut -d ':' -f 3-5`：输出PATH用:分割后第3-5列数据
    - `echo $PATH | cut -c 3,5`：输出PATH的第3、5个字符
    - `echo $PATH | cut -c 3-5`：输出PATH的第3-5个字符
7. `sort`：将每行内容按字典序排序，find命令输出比较随机，可以通过管道sort排序
    - 可以从stdin中读取多行数据
    - 可以从命令行参数中读取文件名列表
8. `xargs`：将stdin中的数据用空格或回车分割成命令行参数
    - `find . -name '*.py' | xargs cat | wc -l`：统计当前目录下所有python文件的总行数

### 查看文件内容

1. more：浏览文件内容
    - 回车：下一行
    - 空格：下一页
    - b：上一页
    - q：退出
2. less：与more类似，功能更全
    - 回车：下一行
    - y：上一行
    - Page Down：下一页
    - Page Up：上一页
    - q：退出
3. head -3 xxx：展示xxx的前3行内容
    - 同时支持从stdin读入内容
4. tail -3 xxx：展示xxx末尾3行内容
    - 同时支持从stdin读入内容

### 用户相关

history：展示当前用户的历史操作。内容存放在`~/.bash_history`中

### 工具

1. md5sum：计算md5哈希值
    - 可以从stdin读入内容
    - 也可以在命令行参数中传入文件名列表；
2. time command：统计command命令的执行时间
3. ipython3：交互式python3环境。可以当做计算器，或者批量管理文件。
4. watch -n 0.1 command：每0.1秒执行一次command命令
5. tar：压缩文件
    - tar -zcvf xxx.tar.gz /path/to/file/*：压缩
    - tar -zxvf xxx.tar.gz：解压缩
6. diff xxx yyy：查找文件xxx与yyy的不同点

### 安装软件

1. sudo command：以root身份执行command命令
2. apt-get install xxx：安装软件
3. pip install xxx --user --upgrade：安装python包