---
title: Python入门笔记（五）
tags:
  - Python入门
categories:
  - Python
mathjax: true
copyright: true
abbrlink: f5296746
date: 2021-02-24 07:05:40
---

## 1.文件读取指针函数seek()用法详解

seek()方法的语法如下：`fileObject.seek(offset, from)`

offset ： 开始的偏移量，也就是代表需要移动偏移的字节数，如果是负数表示从倒数第几位开始。 

from：可选，默认值为 0。给 offset 定义一个参数，表示要从哪个位置开始偏移；0 代表从文件开头开始算起，1 代表从当前位置开始算起，2 代表从文件末尾算起。

<!--more-->

**实例1**

下面将用实例演示一下seek()方法的用法
下面是‘test.txt’文件内容：

```python
0123456789
0123456789
0123456789
123
```

代码：

```python
file = open('test.txt', 'r') #只读方式，文件读取指针指向开头
print(file.tell())           #file.tell()返回当前读取指针
file_data = file.readline()  #读取一行数据
print(file_data)

file.seek(3,0)               #从0开始偏移3个字节
print(file.tell())
file_data = file.readline()  #读取一行数据
print(file_data)
```

结果：

```python
0
0123456789

3
3456789

123456
```

上述结果之间空一行是因为读取了末尾的换行符

**实例2**

下面我再说一个例子，关于文件打开模式为‘a+’情况下的例子
文件跟上一个例子一样，先看代码：

```python
file = open('test.txt', 'a+')  #可读写方式，文件读取指针指向末尾，数据追加在末尾
print(file.tell())             #file.tell()返回当前读取指针
file.write('9876543210\n')     #末尾写入数据
print(file.tell())             #文件写入后返回当前读取指针
file_data = file.readline()    #读取一行数据
print('one' + file_data)       #文件读取指针在末尾，读取数据为空
file.seek(0,0)
print('two' + file.readline()) #文件读取指针在开头，读取第一行数据
12345678
```

结果：

```python
36
48
one
two0123456789
1234
```

没有读取到数据是因为文件读取指针指向了末尾，所以在使用‘a+’模式时，如果要读取数据要利用seek(0,0)设置文件读取指针
‘test.txt’的变化：

```python
0123456789
0123456789
0123456789
9876543210

12345
```

上述这个例子是我学习Python文件操作时遇到的坑，在利用‘a+’模式打开文件并写入数据后，用readline()怎么都无法读取数据，后来了解到是因为文件读取指针在末尾，所以无法读取数据，要用seek(0，0)方法将文件读取指针重置，才可以读取数据。

## 2.小练习

### 2.1 题目简化：

将浮点数num保留整数部分输出。

```python
# solution 1
print("{}".format(int(num)))
# solution 2
print("{:.0f}".format(num)) # 掌握这种做法
# print("{:d}".format(num))这样不行，无法直接将浮点型转换为int
```

### 2.2 python的连续不等式写法：

`1 <= n <=  2`

这种写法在python中是合理的，在其他大多数语言可能不成立。

### 2.3 小例题1

让用户输入一个数字，判断它是否等于553，如果不等于则让用户再次输入，否则退出程序。

```python
n = eval(input())
while (not n.isdigit()) or n != 553:#  错误示范！！！教训！！！
	n = eval(input())
    
n = eval(input())
while (not n.isdigit()) or int(n) != 553: # n是字符串，需要先转换！！
	n = eval(input())
```

### 2.4 小例题2

![image-20210204162557662](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232140233.png)

```python
# solution 1
s = input()
sa = s.split(",")
a = int(sa[0])
b = int(sa[1])
c = int(sa[2])
ls = []
for i in range(c):
	ls.append(a + b * i)
print(ls)
# solution 2
a,b,c = input().split(",")
a,b,c = int(a),int(b),int(c)
ls = [a + b * i for i in range(c)] # 生成列表一步到位
print(ls)
```

### 2.5 小例题3

判断用户输入的一个数是不是回文数。

```python
# solution 1
n = eval(input())
t = n
m = 0
while t:
	m = m*10 + t%10
	t //= 10

if n == m:
	print("是") 
else:
	print("否")
# solution 2
s = input()
if s == s[::-1]:
    print("是")
else:
	print("否")
```

## 3.pass语句

Python pass 是空语句，是为了保持程序结构的完整性。

**pass** 不做任何事情，一般用做占位语句。

在 Python 中有时候会看到一个 def 函数:

```python
def sample(n_samples):
    pass
```

该处的 pass 便是占据一个位置，因为如果定义一个空函数程序会报错，当你没有想好函数的内容是可以用 pass 填充，使程序可以正常运行。（python3中好像可以定义空函数）

## 4.for循环补充小知识

在for-in循环中，不能对`虚拟变量i`进行修改，但是可以对`可迭代对象s`进行修改。

```python
s = "abcdefghijklmn"
for i in s:
    if i == "a":
        i = "s"
print(s)
# abcdefghijklmn
s = "abcdefghijklmn"
for i in s:
    s = "vn"
print(s)
# vn
```

## 5.错题1

以下说法中错误的是（）
A、对已经关闭的文件进行读写操作会默认再次打开文件
B、对文件操作完成后即使不关闭文件也不会报错，所以可以不关闭文件
C、对于非空文本文件，read()返回字符串，readlines()返回列表
D、file = open(filename,’rb’)表示只读、二进制方式打开名为filename的文件

答案选A.    B选项是正确的。

## 6.错题2

1、编写代码将列表ls  = [‘新年都未有芳华，\n’,’ 二月初惊见草芽。\n’,’白雪却嫌春色晚，\n’,’故穿庭树作飞花。’]、
写入a.txt文件内容如下：
新年都未有芳华，
二月初惊见草芽。
白雪却嫌春色晚，
故穿庭树作飞花。

2、读取a.txt中的内容，并输出。输出格式如下(输出为一行)：
     新年都未有芳华，二月初惊见草芽。白雪却嫌春色晚， 故穿庭树作飞花。

```python
# solution for 1
ls  = ['新年都未有芳华，\n','二月初惊见草芽。\n','白雪却嫌春色晚，\n','故穿庭树作飞花。']
f = open("a.txt","w")
f.writelines(ls)
f.close()

f = open("a.txt","w")
for s in ls:
	f.write(s)
f.close()

# solution for 2，不会啊
f = open("a.txt","r")
t = f.readlines()
for line in t:
	print(line.strip("\n"),end="")
f.close()
```

## 7.练习1

二千多年前希腊的天文学家希巴克斯命名十二星座，它们是水瓶座、双鱼座、白羊座、金牛座、
双子座、巨蟹座、狮子座、处女座、天秤座、天蝎座、射手座、摩羯座。给出一个CSV文件（SunSign.csv），
内容示例如下：
序号,星座,开始月日,结束月日,Unicode
1,水瓶座,120,218,9810
2,双色座,219,320,9811
3,白羊座,321,419,9800
4,金牛座,420,520,9801
5,双子座,521,621,9802
…（略）
以第1行为例,120表示1月20日,218表示2月18日,9810是Unicode码。
问题：读入CSV文件中数据，获得用户输入。根据用户输入的星座名称，输出此星座的出生日期范围。
参考输入和输出示例格式如下：
请输入星座中文名称（例如，双子座）：双子座
双子座的生日位于521-621之间

```python
# solution 1
f = open("SunSign.csv","r")
ls = f.readlines()
L = []
for line in ls:
	L.append(line.strip("\n").split(","))
n = input()
for l in L:
	if(l[1] == n):
		print("{}的生日位于{}-{}之间".format(n,l[2],l[3]))
f.close()
# solution 2，巧妙的解法
n = input()
f = open("SunSign.csv","r")
ls = f.readlines()
''' ls : 
['序号,星座,开始月日,结束月日,Unicode\n',
     ...,
'12,魔蝎座,1222,119,9809\n']
'''
for line in ls:
    lt = line.split(",")# 用','分隔产生列表lt
    if s == lt[1]:
        print("{}的生日位于{}-{}之间".format(n,lt[2],lt[3]))
f.close()
```

## 8.错题3

![image-20210222184011849](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232140021.png)

当`fo.write()`完成后，指针指向文件末尾，此时`fo.read()`无法读取到内容。

## 9.练习2

![image-20210306230603132](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232140779.png)
```python
f = open("vacations.csv","r")
ls = f.readlines()
s = input("请输入节假日名称(例如,春节):")
for line in ls:
    lt = line.strip("\n").split(",")
    if s == lt[1]:
        print("{}的假期位于{}-{}之间".format(lt[1],lt[2],lt[3]))
f.close()

# 简化版本
f = open("vacations.csv","r")
s = input("请输入节假日名称(例如,春节):")
for line in f:# 直接遍历文件，也是一行一行读取
    lt = line.strip("\n").split(",")
    if s == lt[1]:
        print("{}的假期位于{}-{}之间".format(lt[1],lt[2],lt[3]))
f.close()
```

##### 10.练习3

使用字典和列表型变量完成某课程的考勤记录统计，某班有 74 名同学，名单由考生目录下文件 name.txt 给出，

某课程第一次考勤数据由考生目录下文件 card.csv 给出。请求出第一次缺勤同学的名单。‪

```python
# solution 1，自己写的，比较复杂
f = open("name.txt","r",encoding = "utf-8")
d = dict()
lt = f.readlines()
for line in lt:
    d[line.strip("\n")] = 0
print(d)
f.close()

ft = open("card.csv","r",encoding = "utf-8")
ls = ft.readlines()
for l in ls:
    if d.get(l.strip("\n").split(",")[0],1) == 0:
        d[l.strip("\n").split(",")[0]] = 1
for key,val in d.items():
    if val == 0:
        print(key)
ft.close()
# solution 2，比较简单
f_name = open("name.txt","r",encoding = "utf-8")
f_card = open("card.csv","r",encoding = "utf-8")

ls = f_name.readlines()
lt = f_card.readlines()
L = []
for line in lt:
    L.append(line.split(',')[0])
for i in ls:
    name = i.strip('\n')
    if name not in L:
        print(name)
f_name.close()
f_card.close()
```
