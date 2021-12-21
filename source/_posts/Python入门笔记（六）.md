---
title: Python入门笔记（六）
tags:
  - Python入门
categories:
  - Python
mathjax: true
copyright: true
abbrlink: 90272c9f
date: 2021-03-16 19:17:57
---

## 1.列表作为函数参数的例子

<!--more-->

```python
def func(x=[],y=[6,7]):
    x.append(8)
    y.append(8)
    return x+y

a,b = [1,2],[3,4]
t = func(x=a)
t = func(y=b)
print(func(),end=";")
```

执行完第7行：

a = [1,2,8] **注意**：a被func函数修改了

b = [3,4] # 不变

x = [1,2,8]

y = [6,7,8]

t = [1,2,8,6,7,8]

执行完第8行：

a = [1,2,8] # 不变

b = [3,4,8]

x = [8] 

y = [3,4,8]

执行完第9行：

func()中传入上一步得到的默认参数x和y

输出[8,8,3,4,8,8];

```python
def func(x=[],y=[6,7]):
    x.append(8)
    print("x:{}".format(x))
    y.append(8)
    print("y:{}".format(y))
    return x+y

a,b = [1,2],[3,4]
t = func(x=a)
t = func(y=b)
print(func(),end=";")
'''x:[1, 2, 8]
y:[6, 7, 8]
x:[8]
y:[3, 4, 8]
x:[8, 8]
y:[6, 7, 8, 8]
[8, 8, 6, 7, 8, 8];'''
```

## 2.python处理空行输入

input()遇到空行、回车即停止，即使没接收到有效输入。

![image-20210317205917723](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232139602.png)

```python
data = input()  # 课程名 考分
...  
while data:
    ...
    data = input()
```

## 3.Python算法刷题

一位python大佬：https://www.acwing.com/user/myspace/index/73331/

输入输出模板参考：（特别详细）https://blog.csdn.net/weixin_44399621/article/details/105760997

**对于输入：**

常见的不同的输入格式：

```Python
5 2
1 3 5 7 2
m,n=map(int,input().strip().split())
nums=list(map(int,input().strip().split()))
```

**对于输出：**

python中最常用的输出是使用format格式。

**二维数组初始化：**

```python
result_table=[[0 for i in range(4)] for i in range(5)]
# 以下是把一个5行4列的数组初始化为0
result_table=[[0 for i in range(4)] for i in range(5)]
# 更简便的做法
s = [0]*n
```

**标准化输入输出：**

例题

- 题目描述
    计算a+b
- 输入描述
    输入包括两个正整数a,b(1<=a, b<=10^9)，输入数据包括多组。
- 输出描述
    输出a+b的结果

```python
输入样例：
1 5
10 20
输出样例：
6
30 
```

```python
# 解法1
while 1:
	try:
		a,b = map(int, input().split())
		print(a+b)
	except:
		break
# 解法2
import sys
for line in sys.stdin:
	if line == '\n':break
	a,b = (int(i) for i in line.split())
	result = a + b
	sys.stdout.write("{}\n".format(result))
# 解法3
import sys
for a in sys.stdin:
	b = a.split()
	print(int(b[0])+int(b[1]))
# 解法4
if __name__=="__main__":
	while True:
		try:
			a,b = map(int,input(),split())
			print(a + b)
		except:
			break
```

1. import语句

    引入模块，import sys引入python**标准库**中的sys.py模块

2. sys.stdin

    - sys.stdin是一个标准化输入的方法。

    - 使用sys.stdin.readline()可以实现标准输入，其中默认输入的格式是字符串，如果是int，float类型则需要强制转换。

    - 与input()区别
        sys.stdin.readline()会将标准输入全部获取，包括末尾的‘\n‘，因此用len计算长度时是把换行符’\n‘计算进去的，而input()获取输入时返回的结果是不包含末尾的换行符’\n‘的。
        此外，input()括号内可以直接填写说明文字，例如：

        `n = int(input('Please input a number:\n'))`

3. 解法4解释

    - `if __name__="__main__"`：
        当.py文件被直接运行时，`if __name__="__main__"`之下的代码块将被运行；
        当.py文件以模块形式被导入时，`if __name__="__main__"`之下的代码块不被运行。
        个人理解：相当于程序入口，方便调用。

==100-days04.循环结构，开始看130道字符串内置方法==

https://github.com/jackfrued/Python-100-Days