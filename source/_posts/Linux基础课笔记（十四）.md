---
title: Linux基础课笔记（十四）
tags:
  - Linux
  - shell
categories:
  - Linux
mathjax: false
copyright: true
abbrlink: '7e038237'
date: 2021-10-10 11:12:27
---

## 1.5：Linux常用命令

参考文章： https://www.linuxprobe.com/basic-learning-02.html。

Linux命令非常多，本节讲解几个常用命令。其他命令依赖于大家根据实际操作环境，边用边查。

<!--more-->

**本文贵在精不在全。**

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
    - chmod 777 xxx：将xxx的权限改成777；
    - chmod +x * -R：给目录下所有文件添加可执行权限，-R表示递归。
    
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

find命令默认递归查找，可以设置查找深度。

1. `grep xxx`：从stdin中读入若干行数据，如果某行中包含xxx，则输出该行；否则忽略该行。
2. `wc`：统计行数、单词数、字节数
    - 既可以从stdin中直接读入内容；也可以在命令行参数中传入文件名列表；
    - `wc -l`：统计行数
    - `wc -w`：统计单词数
    - `wc -c`：统计字节数
3. `tree`：展示当前目录的文件结构
    - `tree /path/to/directory/`：展示某个目录的文件结构
    - `tree -a`：展示隐藏文件
4. `ag xxx`：搜索当前目录下的所有文件，检索xxx字符串，**全文搜索，重要功能！**
5. `cut`：分割一行内容
    - 从stdin中读入多行数据
    - `echo $PATH | cut -d ':' -f 3,5`：输出PATH用:分割后第3、5列数据
    - `echo $PATH | cut -d ':' -f 3-5`：输出PATH用:分割后第3-5列数据
    - `echo $PATH | cut -c 3,5`：输出PATH的第3、5个字符
    - `echo $PATH | cut -c 3-5`：输出PATH的第3-5个字符
6. `sort`：将每行内容按字典序排序，find命令输出比较随机，可以通过管道sort排序
    - 可以从stdin中读取多行数据
    - 可以从命令行参数中读取文件名列表
7. `xargs`：将stdin中的数据用空格或回车分割成命令行参数
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

----

## 作业

创建好作业后，先进入文件夹/home/acs/homework/lesson_7/，然后：
(0) 进入homework_0文件夹，然后：
    [1] 编写可执行shell脚本，文件名为mydu，使得在任意目录执行该脚本后，可以输出当前目录所占用的磁盘空间大小，格式化输出（例如 1K, 234M, 2G）。
    [2] 修改环境变量PATH，使得在任意目录下输入mydu均可执行上述脚本。
(1) 进入homework_1文件夹，然后：
    [1] 给当前目录下的所有文件添加可读权限。
    [2] 统计当前目录下共有多少个以.cpp结尾的文件，将结果写入ans.txt中。
(2) 进入homework_2文件夹，然后：
    [1] 统计当前目录下所有cpp文件的总行数，将结果写入ans1.txt中。
    [2] 遍历当前目录下所有py文件，统计共有多少行包含字符串thrift，将结果写入ans2.txt中。
(3) 进入homework_3文件夹，然后删除当前目录下的所有py文件。
(4) 进入homework_4文件夹，然后：
    [1] 当前目录下包含一个文件scores.txt，文件中有若干行数据，每行数据包含用空格隔开的3个字段：姓名、数学成绩、算法成绩。
    [2] 将scores.txt文件的内容以空格为分隔符分隔成3列，将第一列内容写入names.txt，第二列内容写入mathematics.txt，第三列内容写入algorithm.txt。
    注意：scores.txt文件本身不要修改。
(5) 进入homework_5文件夹，然后：
    [1] 当前目录下包含一个文件scores.txt，文件中有若干行数据，每行数据包含用空格隔开的3个字段：姓名、数学成绩、算法成绩。
    [2] 将scores.txt文件的内容以空格为分隔符分隔成3列，将第一列内容按字典序从小到大排序后写入names.txt。
    注意：scores.txt文件本身不要修改。
(6) 进入homework_6文件夹，然后将scores.txt文件的前5行写入top.txt，后4行写入bottom.txt。
(7) 进入homework_7文件夹，计算scores.txt文件的md5sum，将结果写入ans.txt。注意：ans.txt文件中只包含长度为32的md5sum，不包含其他内容。
(8) 进入homework_8文件夹，然后：
    [1] 将dir_a文件夹压缩为project_a.tar.gz。
    [2] 将project_b.tar.gz压缩文件解压到当前目录。
    注意：不要删除dir_a文件夹和project_b.tar.gz文件。
(9) 进入homework_9文件夹，然后计算 2^112 + 3^78，将结果写入ans.txt中，注意^在这里表示乘方。

```shell
# homework_0
vim mydu     # 注意不要加.sh后缀

***
#! /bin/bash

du -sh
***
# 添加可执行权限
chmod +x mydu
# 在~/.bashrc 末尾添加一行代码
export PATH=/home/acs/homework/lesson_7/homework_0:$PATH
# 然后更新
source ~/.bashrc

# homework_1
chmod +r * -R
find . -name '*.cpp' | wc -l > ans.txt

# homework_2
find . -name '*.cpp' | xargs cat | wc -l > ans1.txt
find . -name '*.py' | xargs cat | grep thrift | wc -l > ans2.txt

# homework_3
find . -name '*.py' | xargs rm

# homework_4
cat scores.txt | cut -d ' ' -f 1 > names.txt
cat scores.txt | cut -d ' ' -f 2 > mathematics.txt
cat scores.txt | cut -d ' ' -f 3 > algorithm.txt

# homework_5
cat scores.txt | cut -d ' ' -f 1 | sort > names.txt

# homework_6
head -5 scores.txt > top.txt
tail -4 scores.txt > bottom.txt

# homework_7
md5sum scores.txt | cut -c 1-32 > ans.txt

# homework_8
tar -zcvf project_a.tar.gz dir_a/*
tar -zxvf project_b.tar.gz

# homework_9
ipython3  # 打python3交互环境

res = 2**112 + 3**78

# 加上 ! 在ipython3环境中执行shell命令
! echo $res > ans.txt  # ! 表示后面的内容是shell脚本

exit # 退出python
```

