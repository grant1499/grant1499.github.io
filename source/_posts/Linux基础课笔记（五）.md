---
title: Linux基础课笔记（五）
tags:
  - Linux
categories:
  - Linux
mathjax: true
copyright: true
abbrlink: 2fe5e09c
date: 2021-09-03 22:13:55
---

## 1.10：逻辑运算符&&和||

注意与**expr命令**中的逻辑关系表达式中的`|`和`&`区分！！！

<!--more-->

- && 表示与，|| 表示或
- 二者具有短路原则：
    `expr1 && expr2`：当expr1为假时，直接忽略expr2
    `expr1 || expr2`：当expr1为真时，直接忽略expr2
- 表达式的`exit code`为0，表示真；为非零，表示假。（与C/C++中的定义相反）

利用短路原则的例子：`hexo c && hexo g`，当第一个命令为真才执行第二个。

## 1.11：test命令

在命令行中输入`man test`，可以查看test命令的用法。

test命令用于判断文件类型，以及对变量做比较。

**test命令用`exit code`返回结果，而不是使用`stdout`。0表示真，非0表示假。**

**expr命令用`stdout`返回结果。如果为逻辑关系表达式，stdout为1表示真，0表示假。**

```shell
test 2 -lt 3  # 为真，返回值为0
echo $?  # 输出上个命令的返回值，输出0
```

```shell
acs@9e0ebfcd82d7:~$ ls  # 列出当前目录下的所有文件
homework  output.txt  test.sh  tmp
acs@9e0ebfcd82d7:~$ test -e test.sh && echo "exist" || echo "Not exist"
exist  # test.sh 文件存在
acs@9e0ebfcd82d7:~$ test -e test2.sh && echo "exist" || echo "Not exist"
Not exist  # testh2.sh 文件不存在
```

### 文件类型判断：

命令格式：`test -e filename`

| 测试参数 | 代表意义     |
| -------- | ------------ |
| -e       | 文件是否存在 |
| -f       | 是否为文件   |
| -d       | 是否为目录   |

### 文件权限判断：

命令格式：`test -r filename`

| 测试参数 | 代表意义       |
| -------- | -------------- |
| -r       | 文件是否可读   |
| -w       | 文件是否可写   |
| -x       | 文件是否可执行 |
| -s       | 是否为非空文件 |

### 整数间的比较：

命令格式：`test $a -eq $b`

| 测试参数           | 代表意义       |
| ------------------ | -------------- |
| -eq                | a是否等于b     |
| -ne                | a是否不等于b   |
| -gt (greater than) | a是否大于b     |
| -lt (less than)    | a是否小于b     |
| -ge                | a是否大于等于b |
| -le                | a是否小于等于b |

### 字符串比较：

注意区分整数比较和字符串比较！

| 测试参数          | 代表意义                                               |
| ----------------- | ------------------------------------------------------ |
| test -z STRING    | 判断STRING是否为空，如果为空，则返回true               |
| test -n STRING    | 判断STRING是否非空，如果非空，则返回true（-n可以省略） |
| test str1 == str2 | 判断str1是否等于str2                                   |
| test str1 != str2 | 判断str1是否不等于str2                                 |

```shell
acs@83ea09fe037b:~$ s1=yxc
acs@83ea09fe037b:~$ s2=abc
acs@83ea09fe037b:~$ s3=yxc
acs@83ea09fe037b:~$ test "$s1" == "$s2" && echo "true" || echo "false"
false
acs@83ea09fe037b:~$ test "$s1" == "$s3" && echo "true" || echo "false"
true
acs@83ea09fe037b:~$ test "$s1" != "$s2" && echo "true" || echo "false"
true
```

### 多重条件判定：

作用和&&,||类似。

命令格式：`test -r filename -a -x filename`

| 测试参数 | 代表意义                                            |
| -------- | --------------------------------------------------- |
| -a       | 两条件是否同时成立                                  |
| -o       | 两条件是否至少一个成立                              |
| !        | 取反。如 test ! -x file，当file不可执行时，返回true |

## 1.12：判断符号[]

[]与test用法几乎一模一样，更常用于if语句中。另外[[]]是[]的加强版，支持的特性更多。

例如：

```shell
[ 2 -lt 3 ]  # 为真，返回值为0
echo $?  # 输出上个命令的返回值，输出0
```

```shell
acs@9e0ebfcd82d7:~$ ls  # 列出当前目录下的所有文件
homework  output.txt  test.sh  tmp
acs@9e0ebfcd82d7:~$ [ -e test.sh ] && echo "exist" || echo "Not exist"
exist  # test.sh 文件存在
acs@9e0ebfcd82d7:~$ [ -e test2.sh ] && echo "exist" || echo "Not exist"
Not exist  # testh2.sh 文件不存在
```

注意：

- []内的每一项都要用**空格隔开**
- 中括号内的变量，最好用**双引号括起来**
- 中括号内的常数，最好用**单或双引号括起来**

例如：

```shell
name="acwing yxc"
[ $name == "acwing yxc" ]  # 错误，等价于 [ acwing yxc == "acwing yxc" ]，参数太多
[ "$name" == "acwing yxc" ]  # 正确
acs@83ea09fe037b:~$ test "4" -lt "7" && echo "true" || echo "false" # 常数用引号括起来
true
acs@83ea09fe037b:~$ test "4" -eq "4" && echo "true" || echo "false"
true
```

## 1.13：判断语句

### if…then形式

类似于C/C++中的if-else语句。

#### 单层if

命令格式：

```shell
if condition # condition必须是一个命令，if接收它的返回值
then
    语句1
    语句2
    ...
fi
# 写成一行
if [ $(ps -ef | grep -c "ssh") -gt 1 ]; then echo "true"; fi

acs@83ea09fe037b:~$ expr 4 \<\= 2 && echo "true" || echo "false"
0 # expr命令不要加反引号，否则是一个值，不是一个命令
false
acs@83ea09fe037b:~$ if expr 4 \>\= 3;then echo "true";fi
1
true
```

```shell
a=3
b=4

if [ "$a" -lt "$b" ] && [ "$a" -gt 2 ]
then
    echo ${a}在范围内
fi
输出：
3在范围内
```

#### 单层if-else

命令格式：

```shell
if condition
then
    语句1
    语句2
    ...
else
    语句1
    语句2
    ...
fi
```

```shell
a=3
b=4

if ! [ "$a" -lt "$b" ]
then
    echo ${a}不小于${b}
else
    echo ${a}小于${b}
fi
输出：
3小于4
```

#### 多层if-elif-elif-else

最后的else也可以不加。

命令格式：

```shell
if condition
then
    语句1
    语句2
    ...
elif condition
then
    语句1
    语句2
    ...
elif condition
then
    语句1
    语句2
else
    语句1
    语句2
    ...
fi
```

```shell
a=4

if [ $a -eq 1 ]
then
    echo ${a}等于1
elif [ $a -eq 2 ]
then
    echo ${a}等于2
elif [ $a -eq 3 ]
then
    echo ${a}等于3
else
    echo 其他
fi
输出：
其他
```

### case-in…esac形式

类似于C/C++中的switch语句。

命令格式：

```shell
case $变量名称 in
    值1)
        语句1
        语句2
        ...
        ;;  # 类似于C/C++中的break
    值2)
        语句1
        语句2
        ...
        ;;
    *)  # 类似于C/C++中的default
        语句1
        语句2
        ...
        ;;
esac
```

```shell
a=4

case $a in
    1)
        echo ${a}等于1
        ;;  # 不能省略
    2)
        echo ${a}等于2
        ;;  
    3)                                                
        echo ${a}等于3
        ;;  
    *)
        echo 其他
        ;;  
esac
输出：
其他
```

