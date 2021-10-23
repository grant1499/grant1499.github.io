---
title: Linux基础课笔记（七）
tags:
  - Linux
  - shell
categories:
  - Linux
mathjax: true
copyright: true
abbrlink: 98ebdcda
date: 2021-09-04 12:07:13
---

## 1.17：文件重定向

每个进程默认打开3个文件描述符：

<!--more-->

- stdin标准输入，从命令行读取数据，文件描述符为0
- stdout标准输出，向命令行输出数据，文件描述符为1
- stderr标准错误输出，向命令行输出数据，文件描述符为2

可以用文件重定向将这三个文件重定向到其他文件中。

### 重定向命令列表

|  命令    |  说明    |
| ---- | ---- |
|   `command > file`   |   将stdout重定向到file中   |
|   `command < file`   |   将stdin重定向到file中   |
|   `command >> file`   |  将stdout以追加方式重定向到file中    |
|   `command n> file`   |  将文件描述符n重定向到file中    |
|   `command n>> file`   | 将文件描述符n以追加方式重定向到file中     |

### 输入和输出重定向

```shell
echo -e "Hello \c" > output.txt  # 将stdout重定向到output.txt中
echo "World" >> output.txt  # 将字符串追加到output.txt中

read str < output.txt  # 从output.txt中读取字符串

echo $str  # 输出结果：Hello World
```

### 同时重定向stdin和stdout

创建bash脚本：

```shell
#! /bin/bash

read a
read b

echo $(expr "$a" + "$b")
```

创建input.txt，里面的内容为：

```shell
3
4
```

执行命令：

```shell
acs@9e0ebfcd82d7:~$ chmod +x test.sh  # 添加可执行权限
acs@9e0ebfcd82d7:~$ ./test.sh < input.txt > output.txt  # 从input.txt中读取内容，将输出写入output.txt中
acs@9e0ebfcd82d7:~$ cat output.txt  # 查看output.txt中的内容
7
```

## 1.18：引入外部脚本

类似于C/C++中的include操作，bash也可以引入其他文件中的代码。

语法格式：

```shell
. filename  # 注意点和文件名之间有一个空格
或
source filename
```

创建test1.sh，内容为：

```shell
#! /bin/bash

name=yxc  # 定义变量name
```

然后创建test2.sh，内容为：

```shell
#! /bin/bash
# 可以用相对路径或者绝对路径
source test1.sh # 或 . test1.sh

echo My name is: $name  # 可以使用test1.sh中的变量
```

执行命令：

```shell
acs@9e0ebfcd82d7:~$ chmod +x test2.sh # 引入的test1文件不需要可执行权限
acs@9e0ebfcd82d7:~$ ./test2.sh 
My name is: yxc
```

## 1.19：Linux作业

3. 作业
    创建好作业后，先进入文件夹/home/acs/homework/lesson_3/，然后：
    (0) 进入homework_0文件夹，编写自动完成lesson_1作业的脚本helper.sh。要求：
        [1] 当前目录下仅包含helper.sh
        [2] helper.sh具有可执行权限
        [3] 在任意路径依次执行下列命令后，lesson_1的作业可以得到满分：
            1) homework 1 create
            2) /home/acs/homework/lesson_3/homework_0/helper.sh
    (1) 进入homework_1文件夹，编写脚本check_file.sh。要求：
        [1] 当前目录下仅包含check_file.sh。
        [2] check_file.sh具有可执行权限。
        [3] check_file.sh接收一个传入参数。格式为 ./check_file.sh file
        [4] 判断传递参数，分别在标准输出中输出如下内容（不包括双引号）：
            1) 如果传入参数个数不是1，则输出一行："arguments not valid"，然后退出，退出状态等于1。
            2) 如果file文件不存在，则输出一行："not exist"，然后退出，退出状态等于2。
            3) 如果file文件存在，则输出分别进行如下5个判断，然后退出，退出状态等于0。
                1] 如果file为普通文件，则输出一行："regulare file"
                2] 如果file为目录（文件夹），则输出一行："directory"
                3] 如果file具有可读权限，则输出一行："readable"
                4] 如果file具有可写权限，则输出一行："writable"
                5] 如果file具有可执行权限，则输出一行："executable"
    (2) 进入homework_2文件夹，编写脚本main.sh。要求：
        [1] 当前目录下仅包含main.sh
        [2] main.sh具有可执行权限
        [3] 该文件从stdin(标准输入)中读取一个整数n
        [4] 在stdout(标准输出)输出斐波那契数列的第n项。即：a[0] = 1, a[1] = 1, a[i] = a[i - 1] + a[i - 2], 求a[n]。
        [5] 数据保证 0 <= n <= 20，脚本不需要判断n的合法性。
    (3) 进入homework_3文件夹，编写脚本main.sh。要求：
        [1] 当前目录下仅包含main.sh
        [2] main.sh具有可执行权限
        [3] 该文件从stdin(标准输入)中读取两行整数n和m
        [4] 在stdout(标准输出)中输出1~n的按字典序从小到大的顺序数第m个全排列，输出一行，用空格隔开所有数，行末可以有多余空格。
        [5] 数据保证 1 <= n <= 10, 1 <= m <= min(100, n!)，脚本不需要判断数据的合法性。
    (4) 进入homework_4文件夹，编写脚本main.sh。要求：
        [1] 当前目录下仅包含main.sh
        [2] main.sh具有可执行权限
        [3] main.sh接收两个传入参数。格式为 ./main.sh input_file output_file
        [4] 从input_file中读取一个正整数n，然后将前n个正整数的平方和写入output_file中
        [5] 数据保证 1 <= n <= 100，脚本不需要判断所有数据的合法性。

```shell
# **homework_0**
# help.sh
#! /bin/bash

path=/home/acs/homework/lesson_1/
homework 1 create

# homework_0
cd ${path}homework_0/
mkdir dir_{a..c}

# homework_1
cd ${path}homework_1/
for i in {a..c}
do
    cp ${i}.txt ${i}.txt.bak
done

# homework_2
cd ${path}homework_2/
for i in {a..c}
do
    mv ${i}.txt ${i}_new.txt
done

# homework_3
cd ${path}homework_3/
for i in {a..c}
do
    mv dir_a/${i}.txt dir_b/
done

# honework_4
cd ${path}homework_4/
rm {a..c}.txt

# homework_5
cd ${path}homework_5/
rm dir_{a..c} -r

# homework_6
cd ${path}homework_6/
mkdir dir_a
mv "task.txt" "dir_a/done.txt"

# homework_7
cd ${path}homework_7/
mkdir dir_{0..2}
for i in {0..2}
do
    for j in {a..c}
    do
        cp ${j}.txt dir_${i}/${j}${i}.txt
    done
done

# homework_8
cd ${path}homework_8/
for i in {a..c}
do
    cd dir_${i} 
    if [ ${i} == "a" ] # 注意中括号表达式中每项都要用空格隔开
    then
        rm a.txt
    elif [ ${i} == "b" ]
    then
        mv b.txt b_new.txt
    elif [ ${i} == "c" ]
    then
        cp c.txt c.txt.bak
    fi
    cd ../ # 注意循环中的cd命令，在本次循环结束要cd回到上一层
done

# homework_9
cd ${path}homework_9/
rm *.txt
# **homework_1**
#! /bin/bash

if [ $# -ne 1 ]
then
    echo "arguments not valid"
    exit 1
elif [ -e "$1" ]
then
    if [ -f "$1" ] #可以简写成 [ -f "$1" ] && echo "regulare file"
    then
        echo "regulare file"
    fi
    if [ -d "$1" ]
    then
        echo "directory"
    fi
    if [ -r "$1" ]
    then
        echo "readable"
    fi
    if [ -w "$1" ]
    then
        echo "writable"
    fi
    if [ -x "$1" ]
    then
        echo "executable"
    fi
    exit 0
else
    echo "not exist"
    exit 2
fi
# **homework_2**
#! /bin/bash

read n

a[0]=1;a[1]=1
for ((i = 2;i <= 20;i++))
do
    a[i]=$((a[i-1] + a[i-2]))
done
echo ${a[n]}
# **homework_3**
# dfs + 剪枝，通过flag标记剪枝，在dfs回溯时知道已找到·答案就提前退出
#! /bin/bash

read n
read m

cnt=0
st=()
path=()
flag=0

for ((i = 1;i <= n;i++))
do
    st[i]=0
done

dfs() {
    if [ $1 -eq $((n+1)) ]
    then
        ((cnt++))
        if [ $cnt == $m ]
        then
            local i=1
            for ((i = 1;i <= n;i ++))
            do
                echo -e "${path[i]} \c "
            done
            echo ""
            flag=1
        fi
        return 0
    fi

    local i=1
    for (( i = 1;i <= n;i++))
    do
        if [ ${st[i]} -eq 0 ]
        then
            st[$i]=1
            path[$1]=${i}
            dfs $(expr $1 + 1)
            if [ $flag -eq 1 ]
            then
                return 0
            fi
            st[$i]=0
        fi
    done
}

dfs 1
# **homework_4**
#! /bin/bash

read n < "$1"
res=0
for ((i = 1;i <= n;i++))
do
    res=$((res + i * i))
done
echo $res > "$2"
```

