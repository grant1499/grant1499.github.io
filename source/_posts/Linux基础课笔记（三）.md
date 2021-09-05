---
title: Linux基础课笔记（三）
tags:
  - Linux
categories:
  - Linux
mathjax: true
copyright: true
abbrlink: f7563cbe
date: 2021-09-01 22:13:45
---

# 1：lesson3

## 1.1：shell概述

Shell 既是一种命令语言，又是一种程序设计语言。

Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

<!--more-->

Shell 脚本（shell script），是一种为 shell 编写的脚本程序。

shell脚本可以直接在命令行中执行，也可以将一套逻辑组织成一个文件，方便复用。

Linux中常见的shell脚本有很多种，常见的有：

- Bourne Shell(/usr/bin/sh或/bin/sh)
- Bourne Again Shell(/bin/bash)
- C Shell(/usr/bin/csh)
- K Shell(/usr/bin/ksh)
- zsh
- …

这些shell脚本语法大致相同，但还是有些区别！

Linux系统中一般默认使用bash，所以接下来讲解bash中的语法。

文件开头需要写`#! /bin/bash`，指明bash为脚本解释器。

---

shell内容很多很细，参考[y总讲义](https://www.acwing.com/file_system/file/content/whole/index/content/2855883/)。

还有菜鸟教程： https://www.runoob.com/linux/linux-shell.html。

**学习技巧**

不要死记硬背，遇到含糊不清的地方，可以在AC Terminal里实际运行一遍。

## 1.2：第一个shell脚本

新建`test.sh`，写入内容：

```shell
#! /bin/bash
echo "hello,world!" # 也可以不加双引号
```

**如何运行？**

有两种方式。

1. 作为可执行文件。

```shell
acs@9e0ebfcd82d7:~$ chmod +x test.sh  # 使脚本具有可执行权限
acs@9e0ebfcd82d7:~$ ./test.sh  # 当前路径下执行，一定要带./
Hello World!  # 脚本输出
acs@9e0ebfcd82d7:~$ /home/acs/test.sh  # 绝对路径下执行
Hello World!  # 脚本输出
acs@9e0ebfcd82d7:~$ ~/test.sh  # 家目录路径下执行
Hello World!  # 脚本输出
```

注意，在当前目录下执行，一定要写成 **./test.sh**，而不是 **test.sh**。

2. 用解释器执行。不需要有可执行权限。

```shell
acs@9e0ebfcd82d7:~$ bash test.sh
Hello World!  # 脚本输出
```

## 1.3：注释

单行注释：每行中#之后的内容均是注释。和python一样。

```shell
# 这是一行注释
echo 'Hello World'  #  这也是注释
```

多行注释：

```shell
:<<EOF
第一行注释
第二行注释
第三行注释
EOF
```

其中EOF可以换成其它任意字符串。例如：

```shell
:<<abc
第一行注释
第二行注释
第三行注释
abc

:<<!
第一行注释
第二行注释
第三行注释
!
```

## 1.4：变量

### 定义变量：

定义变量时，变量名不加美元符号。

> 注意，**变量名和等号之间不能有空格**，这可能和你熟悉的所有编程语言都不一样。

```shell
name1='yxc'  # 单引号定义字符串
name2="yxc"  # 双引号定义字符串
name3=yxc    # 也可以不加引号，同样表示字符串
```

已定义的变量，可以被重新定义。

### 使用变量：

使用变量，需要加上`$`符号，或者`${}`符号。花括号是可选的，主要为了帮助解释器识别变量边界。

推荐给所有变量加上花括号，这是个好的编程习惯。

```shell
name=yxc
echo $name  # 输出yxc
echo ${name}  # 输出yxc
echo ${name}acwing  # 输出yxcacwing
```

### 只读变量：

使用`readonly`或者`declare`可以将变量变为只读。

```shell
name=yxc
readonly name
declare -r name  # 两种写法均可

name=abc  # 会报错，因为此时name只读
```

补充一个小知识，使用type命令可以显示命令的类型信息。

```shell
acs@83ea09fe037b:~$ type ls
ls is aliased to `ls --color=auto'
acs@83ea09fe037b:~$ type cd
cd is a shell builtin
acs@83ea09fe037b:~$ type readonly
readonly is a shell builtin
```

### 删除变量：

`unset`可以删除变量。

变量被删除后不能再次使用。unset 命令不能删除只读变量。

```shell
name=yxc
unset name
echo $name  # 输出空行
```

### 变量类型：

1. 自定义变量（局部变量）
    子进程不能访问的变量
2. 环境变量（全局变量）
    子进程可以访问的变量

解释：

- **1) 局部变量** 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
- **2) 环境变量** 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。

```shell
acs@83ea09fe037b:~$ name=yxc
acs@83ea09fe037b:~$ echo ${name}
yxc
acs@83ea09fe037b:~$ bash  # bash命令打开一个shell子进程
acs@83ea09fe037b:~$ echo ${name} # name是局部变量，没有值

acs@83ea09fe037b:~$ exit # exit命令退回上一个shell进程
exit
acs@83ea09fe037b:~$ echo ${name} # 局部变量name只能在当前进程访问
yxc
```

自定义变量改成环境变量：

```shell
acs@9e0ebfcd82d7:~$ name=yxc  # 定义变量
acs@9e0ebfcd82d7:~$ export name  # 第一种方法
acs@9e0ebfcd82d7:~$ declare -x name  # 第二种方法
```

环境变量改为自定义变量：

```shell
acs@9e0ebfcd82d7:~$ export name=yxc  # 定义环境变量
acs@9e0ebfcd82d7:~$ declare +x name  # 改为自定义变量
```

**关于declare命令的补充：**

Linux declare命令用于声明 shell 变量。

- +/- 　"-"可用来指定变量的属性，"+"则是取消变量所设的属性。
- r 　将变量设置为只读。
- x 　指定的变量会成为环境变量，可供shell以外的程序来使用。

### 字符串：

字符串可以用单引号，也可以用双引号，也可以不用引号。

单引号与双引号的区别：

- 单引号中的内容会原样输出，不会执行、不会取变量，不会转义；

- 双引号中的内容可以执行、可以取变量，会转义；

```shell
name=yxc  # 不用引号
echo 'hello, $name \"hh\"'  # 单引号字符串，输出 hello, $name \"hh\"
echo "hello, $name \"hh\""  # 双引号字符串，输出 hello, yxc "hh"
```

获取字符串长度：

```shell
name="yxc"
echo ${#name}  # 输出3，在花括号最前面加上"#"
```

提取子串：

```shell
name="hello, yxc"
echo ${name:0:5}  # 提取从0开始的5个字符
```

### 文件参数变量：

在执行shell脚本时，可以向脚本传递参数。`$1`是第一个参数，`$2`是第二个参数，以此类推。特殊的，`$0`是执行的文件名（包含路径）。例如：

创建文件test.sh：

```shell
#! /bin/bash

echo "文件名："$0
echo "第一个参数："$1
echo "第二个参数："$2
echo "第三个参数："$3
echo "第四个参数："$4
```

然后执行该脚本：

```shell
acs@9e0ebfcd82d7:~$ chmod +x test.sh 
acs@9e0ebfcd82d7:~$ ./test.sh 1 2 3 4
文件名：./test.sh
第一个参数：1
第二个参数：2
第三个参数：3
第四个参数：4
```

其它参数相关变量：

| 参数        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| $#          | 代表文件传入的参数个数，如上例中值为4                        |
| $*          | 由所有参数构成的用空格隔开的字符串，如上例中值为`"$1 $2 $3 $4"` |
| $@          | 每个参数分别用双引号括起来的字符串，如上例中值为`"$1" "$2" "$3" "$4"` |
| $$          | 脚本当前运行的进程ID                                         |
| $?          | 上一条命令的退出状态（注意不是stdout，而是exit code）。0表示正常退出，其他值表示错误 |
| $(command)  | 返回command这条命令的stdout                                  |
| \`command\` | 返回command这条命令的stdout                                  |

在Markdown中显示转义字符（如#*`）,可以使用反斜杠 \ 来进行转义。

`exit code`类似C++中main函数`return 0`，`stdout`类似C++中cout的内容。

```shell
# test.sh内容
#! /bin/bash

echo $(ls)
echo `ls`
# 执行脚本
acs@83ea09fe037b:~$ ./test.sh 
homework test.sh
homework test.sh
```

## 1.5：数组

数组中可以存放多个不同类型的值，只支持一维数组，初始化时不需要指明数组大小。

**数组下标从0开始。**

### 定义：

数组用小括号表示，元素之间用空格隔开。例如：

```shell
array=(1 abc "def" yxc) # 都是字符串，可以不加引号，加单、双引号
```

也可以直接定义数组中某个元素的值：

```shell
array[0]=1
array[1]=abc
array[2]="def"
array[3]=yxc
```

### 读取数组中某个元素的值：

格式：`${array[index]}`

```shell
array=(1 abc "def" yxc)
echo ${array[0]}
echo ${array[1]}
echo ${array[2]}
echo ${array[3]}
```

### 读取整个数组：

格式：

```shell
${array[@]}  # 第一种写法
${array[*]}  # 第二种写法
```

### 数组长度：

类似于字符串在花括号最前面加上`#`。

```shell
${#array[@]}  # 第一种写法
${#array[*]}  # 第二种写法
```

