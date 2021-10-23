---
title: Linux基础课笔记（四）
tags:
  - Linux
  - shell
categories:
  - Linux
mathjax: true
copyright: true
abbrlink: 407b25db
date: 2021-09-01 22:13:55
---

## 1.6：expr命令

expr命令用户求表达式的值，格式为：`expr 表达式`。

expr命令详解： https://www.cnblogs.com/f-ck-need-u/p/7231832.html。

<!--more-->

### 表达式说明：

- 用空格隔开每一项
- 其实expr命令自带输出，`expr xxx `等价于`echo $(expr xxx)`等价于`` echo `expr xxx` ``
- 用反斜杠放在shell特定的字符前面（发现表达式运行错误时，可以试试转义`\`）
- 对包含空格和其他特殊字符的字符串要用引号括起来
- expr会在stdout中输出结果。（可以不加echo）如果为逻辑关系表达式，则结果为真，stdout为1，否则为0。
- expr的exit code：如果为逻辑关系表达式，则结果为真，exit code为0，否则为1。

### 字符串表达式：

1. `length STRING`：返回`STRING`的长度
2. `index STRING CHARSET`：`CHARSET`中任意单个字符在STRING中最前面的字符位置，**下标从1开始**。如果在`STRING`中完全不存在`CHARSET`中的字符，则返回0。
3. `substr STRING POSITION LENGTH`：返回`STRING`字符串中从`POSITION`开始，**下标从1开始**，长度最大为`LENGTH`的子串。如果`POSITION`或`LENGTH`为负数，0或非数值，则返回空字符串。

```shell
#!/bin/bash

val=`expr 2 + 2`
echo "两数之和为 : $val"

str="Hello World!"

echo `expr length ${str}` # expr: syntax error: unexpected argument 'World!'
# str中间有空格，会认为是2个参数，出错，需要加上引号
echo `expr length "$str"`  # ``不是单引号，表示执行该命令，输出12
# 被反引号包括的表达式可以写成$(...)，等价的，例如：echo $(expr length "$str")
echo `expr index "$str" aWd`  # 输出7，下标从1开始
echo `expr substr "$str" 2 3`  # 输出 ell
```

**补充：**shell语法中`;`可以用于区分单行的多条语句。

**"index string chars"用法解释。**

该表达式是从string中搜索chars中某个字符的位置，这个字符是string中最靠前的字符。例如：

```shell
[root@xuexi ~]# expr index abcde dec
3
```

该命令将对字符串"dec"逐字符分解，首先分解得到第一个字符d，从abcde中搜索到d的位置为4，再分解得到第二个字符e，该字符在abcde中的位置为5，最后得到的字符是c，该字符在abcde中的位置为3。其中3是最靠前的字符，所以命令返回的结果为3。

补充关于`$()`和 `` ` ` ``：
在 bash shell 中，`$( )` 与 `` ` ` ``(反引号) 都是用来做命令替换用(commandsubstitution)的。可以用来给变量赋值，`a=$(expr $a + 3)`，等价于C++中`a += 3`。

例如 `version=$(uname -r)`和`version=uname -r`都可以是version得到内核的版本号。

各自的优缺点：

1. `` ` ` ``基本上可用在全部的 unix shell 中使用，若写成 shell script ，其移植性比较高。但反单引号容易打错或看错。注意：反引号不能嵌套使用。
2. `$()`并不是所有shell都支持。

### 整数表达式：

`expr`支持普通的算术操作，算术表达式优先级低于字符串表达式，高于逻辑关系表达式。

注意用空格隔开每项。

+ `+ -`
加减运算。两端参数会转换为整数，如果转换失败则报错。

* `* / %`
乘，除，取模运算。两端参数会转换为整数，如果转换失败则报错。乘法运算需要转义。

- () 可以提高优先级，但需要用反斜杠转义。

```shell
a=3
b=4

echo `expr $a + $b`  # 输出7
echo `expr $a - $b`  # 输出-1
echo `expr $a \* $b`  # 输出12，*需要转义
echo `expr $a / $b`  # 输出0，整除
echo `expr $a % $b` # 输出3
echo `expr \( $a + 1 \) \* \( $b + 1 \)`  # 输出20，值为(a + 1) * (b + 1)
```

### 逻辑关系表达式：

- `|` 如果第一个参数非空且非0，则返回第一个参数的值，否则返回第二个参数的值，但要求第二个参数的值也是非空或非0，否则返回0。如果第一个参数是非空或非0时，不会计算第二个参数。**短路原则。**
- `&` 如果两个参数都非空且非0，则返回第一个参数，否则返回0。如果第一个参为0或为空，则不会计算第二个参数。
- `< <= = == != >= >` 比较两端的参数，如果为true，则返回1，否则返回0。”==”是”=”的同义。词。”expr”首先尝试将两端参数转换为整数，并做算术比较，如果转换失败，则按字符集排序规则做字符比较。
- `()` 可以提高优先级，但需要用反斜杠转义。
- 用单引号括起来特殊字符比转义方便一点。

```shell
a=3
b=4

echo `expr $a \> $b`  # 输出0，>需要转义
echo `expr $a '<' $b`  # 输出1，也可以将特殊字符用引号引起来
echo `expr $a '>=' $b`  # 输出0
echo `expr $a \<\= $b`  # 输出1

c=0
d=5

echo `expr $c \& $d`  # 输出0
echo `expr $a \& $b`  # 输出3
echo `expr $c \| $d`  # 输出5
echo `expr $a \| $b`  # 输出3
```

## 1.7：read命令

read命令用于从标准输入中读取**单行数据**。当读到文件结束符（EOF）时，exit code为1，否则为0。

**参数说明**

- -p: 后面可以接提示信息

- -t：后面跟秒数，定义输入字符的等待时间，超过等待时间后会自动忽略此命令

```shell
acs@9e0ebfcd82d7:~$ read name  # 读入name的值
acwing yxc  # 标准输入
acs@9e0ebfcd82d7:~$ echo $name  # 输出name的值
acwing yxc  #标准输出
acs@9e0ebfcd82d7:~$ read -p "Please input your name: " -t 30 name  # 读入name的值，等待时间30秒
Please input your name: acwing yxc  # 标准输入
acs@9e0ebfcd82d7:~$ echo $name  # 输出name的值
acwing yxc  # 标准输出
```

## 1.8：echo命令

echo用于输出字符串。

### 显示普通字符串：

```shell
echo "Hello AC Terminal"
echo Hello AC Terminal  # 引号可以省略，也可以用单引号
```

### 显示转义字符：

```shell
echo "\"Hello AC Terminal\""  # 注意只能使用双引号，如果使用单引号，则不转义
echo \"Hello AC Terminal\"  # 也可以省略双引号
```

### 显示变量：

```shell
name=yxc
echo "My name is $name"  # 输出 My name is yxc
echo My name is $name  # 输出 My name is yxc
```

### 显示换行：

```shell
echo -e "Hi\n"  # -e 开启转义
echo "acwing"
输出：
Hi

acwing
# 输入：man echo可以找到打开-e后支持的转义符号，有\n,\c,\t等等，这些和转义字符\',\*等不一样
```

### 显示不换行：

```shell
echo -e "Hi \c" # -e 开启转义 \c 不换行
echo "acwing"
输出：
Hi acwing
```

注意：想通过-e显示转义符号\n,\c,\t等等，不能省略字符串的双引号！！！

### 显示结果定向至文件：

```shell
echo "Hello World" > output.txt  # 将内容以覆盖的方式输出到output.txt中，没有会新建文件
```

### 原样输出字符串，不进行转义或取变量(用单引号)：

```shell
name=acwing
echo '$name\"'
# 输出：$name\"
```

### 显示命令的执行结果：

```shell
echo `date` # 也就是获取命令的stdout
# 输出：Fri Sep 3 21:51:52 CST 2021
```

## 1.9：printf命令

printf命令用于格式化输出，类似于C/C++中的printf函数。

printf 命令模仿 C 程序库（library）里的 printf() 程序。

printf 由 POSIX 标准所定义，因此使用 printf 的脚本比使用 echo 移植性好。

**默认的 printf 不会像 `echo` 自动添加换行符，我们可以手动添加 `\n`。**

命令格式：`printf format-string [arguments...]`

脚本内容：

```shell
printf "%10d!\n" 123  # 占10位，右对齐
printf "%-10.2f!\n" 123.123321  # 占10位，保留2位小数，左对齐
printf "My name is %s\n" "yxc"  # 格式化输出字符串
printf "%d * %d = %d\n"  2 3 `expr 2 \* 3` # 表达式的值作为参数
```

输出结果：

```shell
       123!
123.12    !
My name is yxc
2 * 3 = 6
```

