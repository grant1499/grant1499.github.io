---
title: Linux基础课笔记（六）
tags:
  - Linux
categories:
  - Linux
mathjax: true
copyright: true
abbrlink: 4aebab45
date: 2021-09-04 08:19:11
---

## 1.14：循环语句

### for…in…do…done

命令格式：

<!--more-->

```shell
for var in val1 val2 val3
do
    语句1
    语句2
    ...
done
```

示例1，输出a 2 cc，每个元素一行：

```shell
for i in a 2 cc
do
    echo $i
done
```

示例2，输出当前路径下的所有文件名，每个文件名一行：

```shell
for file in `ls` # ``或者$()用于提取命令的stdout
do
    echo $file
done
```

示例3，输出1-10

```shell
for i in $(seq 1 10)
do
    echo $i
done
# seq命令用于获取一个序列，支持整数、浮点数，不支持字符
# 语法：seq [选项]... 起始值 增量 结束值，增量可以省略，默认为1
```

示例4，使用{1..10} 或者 {a..z}，正向反向都可以

```shell
for i in {a..z}
do
    echo $i
done
```
###  `(())`命令

参考资料： http://c.biancheng.net/view/2480.html。

双小括号 (( )) 是 Bash Shell 中专门用来进行整数运算的命令，它的效率很高，写法灵活，是企业运维中常用的运算命令。

**效率很高，推荐使用。**非常灵活，`(())`命令表达式内容可以加或不加空格，也不需要转义。可以替代expr。

语法格式：`((表达式)) 近似于 expr 表达式`，`$((表达式)) 近似于 $(expr 表达式)`。

在 (( )) 中使用变量无需加上`$`前缀，(( )) 会自动解析变量名。

```shell
((a=10+66) # 计算完成后给变量赋值
((b=a-15))
a=$((10+66) # 可以在 (( )) 前面加上$符号获取 (( )) 命令的执行结果，也即获取整个表达式的值
b=$((a-15))
[c.biancheng.net]$ echo $((3<8))  #3<8 的结果是成立的，因此，输出了 1，1 表示真
1
[c.biancheng.net]$ echo $((8<3))  #8<3 的结果是不成立的，因此，输出了 0，0 表示假。
0
[c.biancheng.net]$ echo $((8==8)) #判断是否相等。
1
[c.biancheng.net]$ if ((8>7&&5==5))
> then
> echo yes
> fi
yes
```

### for ((…;…;…)) do…done

命令格式：注意和C/C++的区别，最外层是**两个小括号**！！！

```shell
for ((expression; condition; expression))
do
    语句1
    语句2
done
```

示例，输出1-10，每个数占一行：

```shell
for ((i=1; i<=10; i++))
do
    echo $i
done
```

shell中也有**只针对变量**的自增自减运算符。

- 前置自增或前置自减会首先修改变量的值，然后再将变量的值传递出去；
- 后置自增或后置自减会首先将变量的值传递出去，然后修改变量的值；
- 自增符号为`++`，自减符号为`--`，且操作对象只能是变量，不能是常数或表达式。
- `a=10;((a++));echo $a`
- 补充：shell脚本整型变量自增方式： https://blog.csdn.net/yumushui/article/details/53469845。


### while…do…done

命令格式：

```shell
while condition
do
    语句1
    语句2
    ...
done
```

示例，文件结束符（EOF）为`Ctrl+d`，输入文件结束符后read指令返回false。

```shell
while read name
do
    echo $name
done
```

### until…do…done

当条件为真时结束。与while是反着来的。

命令格式：

```shell
until condition
do
    语句1
    语句2
    ...
done
```

示例，当用户输入yes或者YES时结束，否则一直等待读入。

```shell
until [ "${word}" == "yes" ] || [ "${word}" == "YES" ]
do
    read -p "Please input yes/YES to stop this program: " word
done
```

### break命令

跳出当前一层循环，注意与C/C++不同的是：break不能跳出case语句。

示例：

```shell
while read name
do
    for ((i=1;i<=10;i++))
    do
        case $i in
            8)
                break # 跳出for循环
                ;;
            *)
                echo $i
                ;;
        esac
    done
done
```

该示例每读入非EOF的字符串，会输出一遍1-7。

该程序可以输入`Ctrl+d`文件结束符来结束，也可以直接用`Ctrl+c`杀掉该进程。

### continue命令

跳出当前循环。

示例：

```shell
for ((i=1;i<=10;i++))
do
    if [ `expr $i % 2` -eq 0 ]  # 等价于 if [ $((i % 2)) -eq 0]
    then
        continue
    fi
    echo $i
done
```

该程序输出1-10中的所有奇数。

### 死循环的处理方式

如果AC Terminal可以打开该程序，则输入`Ctrl+c`即可。

否则可以直接关闭进程：

- 使用top命令找到进程的PID
- 输入`kill -9 PID`即可关掉此进程

## 1.15：函数

bash中的函数类似于C/C++中的函数，但return的返回值与C/C++不同，返回的是`exit code`，取值为0-255，0表示正常结束。

如果想获取函数的输出结果，可以通过echo输出到`stdout`中，然后通过`$(function_name)`来获取`stdout`中的结果。（也可以是``` ` ）

函数的return值可以通过`$?`来获取。

命令格式：

```shell
[function] func_name() {  # function关键字可以省略
    语句1
    语句2
    ...
}
```

### 不获取 return值和stdout值

```shell
func() {
    name=yxc
    echo "Hello $name"
}

func # 运行函数和执行命令差不多
输出：
Hello yxc
```

### 获取 return值和stdout值

不写return时，默认return 0。

```shell
func() {
    name=yxc
    echo "Hello $name"

    return 123
}

output=$(func)
ret=$?

echo "output = $output"
echo "return = $ret"
输出：
output = Hello yxc
return = 123
```

### 函数的输入参数

在函数内，`$`1表示第一个输入参数，`$2`表示第二个输入参数，依此类推。

注意：函数内的`$0`仍然是文件名，而不是函数名。

```shell
func() {  # 递归计算 $1 + ($1 - 1) + ($1 - 2) + ... + 0
    if [ $1 -le 0 ] # 递归边界
    then
        echo 0
        return 0
    fi  

    sum=$(func $(expr $1 - 1))
    echo $(expr $sum + $1)
}
# 函数传递参数类似shell脚本传递参数
echo $(func 10) # 55
# stdout：func 10 == func 9 + 10 == func 8 + 9 + 10 == ... == 55
```

### 函数内的局部变量

可以在函数内定义局部变量，作用范围仅在当前函数内。

可以在递归函数中定义局部变量。

命令格式：`local 变量名=变量值`

```shell
#! /bin/bash

func() {
    local name=yxc
    echo $name
}
func

echo $name
```

输出结果：

```shell
yxc

```

第一行为函数内的name变量，第二行为函数外调用name变量，会发现此时该变量不存在。

## 1.16：exit命令

exit命令用来退出当前shell进程，并返回一个退出状态；使用$?可以接收这个退出状态。

exit命令可以接受一个整数值作为参数，代表退出状态。如果不指定，默认状态值是 0。

exit退出状态只能是一个介于 0~255 之间的整数，其中只有 0 表示成功，其它值都表示失败。

---

创建脚本test.sh，内容如下：

```shell
#! /bin/bash

if [ $# -ne 1 ]  # 如果传入参数个数等于1，则正常退出；否则非正常退出。
then
    echo "arguments not valid"
    exit 1
else
    echo "arguments valid"
    exit 0
fi
```

执行该脚本：

```shell
acs@9e0ebfcd82d7:~$ chmod +x test.sh 
acs@9e0ebfcd82d7:~$ ./test.sh acwing
arguments valid
acs@9e0ebfcd82d7:~$ echo $?  # 传入一个参数，则正常退出，exit code为0
0
acs@9e0ebfcd82d7:~$ ./test.sh 
arguments not valid
acs@9e0ebfcd82d7:~$ echo $?  # 传入参数个数不是1，则非正常退出，exit code为1
1
```

