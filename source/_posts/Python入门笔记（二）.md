---
title: Python入门笔记（二）
tags:
  - Python入门
categories:
  - Python
mathjax: true
copyright: true
abbrlink: '60841836'
date: 2021-02-24 07:05:12
---

## 1.进阶赋值方法

- 同时给多个变量赋同一个值：

`a = b = c = 100`

- 同时给多个变量赋不同的值：

`a , b , c  = 1, 2 , 3`

<!--more-->

## 2.逻辑推理练习

```python
4.0 == 4 # True
bool("1"),bool("0") # True,True
#这里是两个字符串，注意是"0"，不是数字0
str(32) # '32' ,这里直接变成字符串，不作其他改动，和ASCII码无关
int("3.42") # 报错
bool(-1)，bool(""),bool(0) # True,False,False
"wrqq" > "acd" # True
```

- bool函数进行转换时，其结果取决于传入参数与True和 False的等价关系。

只需记住一点即可：**数字0，空字符串，None在条件判断语句中等价于 False**，其他数值都等价于True。bool函数在做数据类型转换时遵循该原则。

- 用加号连接字符串：不会自动加空格；用逗号连接字符串：自动加空格。

  ```python
  >>> print('a'+'b'+'c')
  abc
  print('a','b','c')
  a b c
  ```

- 字符串大小比较规则

两个字符串在比较大小时，比的不是长度，而是内容，逐个字符按<u>Unicode码</u>比较

字符串左对齐后，逐个字符依次比较，直到可以分出胜负

```python
# 计算16开根
# solution 1，只适用于正数
16 ** 0.5
4.0
# solution 2，可以用于正数，负数和复数
import math
math.sqrt(16)
4.0
```

## 3.列表（List）

列表的正向和逆向索引与字符串基本相同。

可以直接对列表的数据进行修改和更新。用del语句删除列表元素。

```python
list = ['Google', 'Runoob', 1997, 2000]
print ("第三个元素为 : ", list[2])
list[2] = 2001
print ("更新后的第三个元素为 : ", list[2])
del list[2]
print ("删除第三个元素 : ", list)
```

### 3.1 列表的操作函数

列表类型继承序列类型特点，有一些通用的操作函数

| **操作函数** | **描述**                             |
| ------------ | ------------------------------------ |
| len(ls)      | 列表ls的元素个数（长度)              |
| min(ls)      | 列表ls中的最小元素(内部数据可比较)   |
| max(ls)      | 列表ls中的最大元素  (内部数据可比较) |
| list(x)      | 将x转变成列表类型                    |

### 3.2 列表的操作方法

基本语法：<列表变量>.<方法名称>(<方法参数>)

| **方法**                            | **描述**                                                     |
| ----------------------------------- | ------------------------------------------------------------ |
| ls.append(x)                        | 在列表ls最后增加一个元素x                                    |
| ls.insert(index, x)                 | 在列表ls第index位置增加元素x，无返回值                       |
| ls.clear()                          | 删除ls中所有元素                                             |
| ls.pop(index)                       | 将列表ls中第index项元素取出并删除该元素，默认最后一个元素，并且返回该元素的值 |
| ls.remove(x)                        | 将列表中出现的第一个元素x删除，无返回值                      |
| ls.reverse()                        | 列表ls中元素反转                                             |
| ls.copy()                           | 生成一个新列表，复制ls中所有元素                             |
| ls.index(obj)                       | 从列表中找出某个值第一个匹配项的索引位置                     |
| ls.count(obj)                       | 统计某个元素在列表中出现的次数                               |
| list.sort( key=None, reverse=False) | reverse – 排序规则，reverse = True 降序， reverse = False 升序（默认）。 |

### 3.3 列表的脚本操作符

| Python 表达式                         | 结果                         | 描述                 |
| ------------------------------------- | ---------------------------- | -------------------- |
| len([1, 2, 3])                        | 3                            | 长度                 |
| [1, 2, 3] + [4, 5, 6]                 | [1, 2, 3, 4, 5, 6]           | 组合                 |
| [‘Hi!’] * 4                           | [‘Hi!’, ‘Hi!’, ‘Hi!’, ‘Hi!’] | 重复                 |
| 3 in [1, 2, 3]                        | True                         | 元素是否存在于列表中 |
| for x in [1, 2, 3]: print(x, end=” “) | 1 2 3                        | 迭代                 |

```python
>>>L=['Google', 'Runoob', 'Taobao']
>>> L[2]
'Taobao'
>>> L[-2]
'Runoob'
>>> L[1:]
['Runoob', 'Taobao']
>>>squares = [1, 4, 9, 16, 25]
>>> squares += [36, 49, 64, 81, 100]
>>> squares
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
# list嵌套
>>>a = ['a', 'b', 'c']
>>> n = [1, 2, 3]
>>> x = [a, n]
>>> x
[['a', 'b', 'c'], [1, 2, 3]]
>>> x[0]
['a', 'b', 'c']
>>> x[0][1]
'b'
# 列表
vowels = ['e', 'a', 'u', 'o', 'i']
 
# 降序
vowels.sort(reverse=True)
 
# 输出结果
print ( '降序输出:', vowels )
```

```python
name = list("python")
print(name)
name[2:] = list("abc")
print(name)

numbers = [1, 5]
numbers[1:1] = [2, 3, 4]  # [1:1]表示1与5之间的位置
print(numbers)

numbers[1:4] = []
print(numbers)

l = [1,2,3,4]
l.insert(2,'a') # 在索引下标2之前插入'a'
print(l)
# 注意区别remove和pop
l = [1,2,3,4,5]
l.remove(1) # 无返回值，删除第一个元素1
print(l)
l.pop(2)# 返回索引下标为2的元素值并删除它
print(l)
# ['p', 'y', 't', 'h', 'o', 'n']
# ['p', 'y', 'a', 'b', 'c']
# [1, 2, 3, 4, 5]
# [1, 5]
# [1,2,'a',3,4]
# [2, 3, 4, 5]
# [2, 3, 5]
```

Question: 已知列表ls = [1,3,5,7,9,5]，去掉列表中的重复值。

Answer: 

```python
ls = [1,3,5,7,9,5]
s = set(ls) # 利用集合的无重复性
ls = list(s)
print(ls)
```

## 4.字典（Dict）

字典是另一种可变容器模型，且可存储任意类型对象。

字典的每个键值 key=>value  对用冒号 :  分割，每个对之间用逗号(**,**)分割，整个字典包括在花括号 {} 中 .

键必须是唯一的，但值则不必。

值可以取任何数据类型，但键必须是不可变的，如字符串，数字。key可以使用元组，而不能使用列表、字典。

```python
dict = {'name': 'runoob', 'likes': 123, 'url': 'www.runoob.com'}
dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}
print ("dict['Name']: ", dict['Name'])# dict['Name']:  Runoob
print ("dict['Age']: ", dict['Age']) # dict['Age']:  7
dict['Age'] = 8               # 更新 Age
dict['School'] = "菜鸟教程"  # 添加信息

del dict['Name'] # 删除键 'Name'
dict.clear()     # 清空字典
del dict         # 删除字典
# 不允许同一个键出现两次。创建时如果同一个键被赋值两次，后一个值会被记住
# 键必须不可变，所以可以用数字，字符串或元组充当，而用列表就不行
```

### 4.1 字典的操作函数

| **操作函数** | **描述**                |
| ------------ | ----------------------- |
| len(d)       | 字典d的元素个数（长度） |
| min(d)       | 字典d中键的最小值       |
| max(d)       | 字典d中键的最大值       |
| dict()       | 生成一个空字典          |

### 4.2 字典的操作方法

基本语法：`<字典变量>.<方法名称>(<方法参数>)`

| **操作方法**        | **描述**                                               |
| ------------------- | ------------------------------------------------------ |
| d.keys()            | 返回所有的键信息                                       |
| d.values()          | 返回所有的值信息                                       |
| d.items()           | 返回所有的键值对                                       |
| d.get(key, default) | 键存在则返回相应值，否则返回默认值                     |
| d.pop(key, default) | 键存在则返回相应值，同时删除键值对，否则返回默认值     |
| d.popitem()         | 随机从字典中取出一个键值对，以元组(key, value)形式返回 |
| d.clear()           | 删除所有的键值对                                       |

### 创建字典

```python
d = {"a":1,"b":2} # 一般的方式创建字典
print(d)
data = [("a",1),("b",2)] # 根据其他序列新建字典
print(data)
d = dict(data)
print(d)
d = dict(lilei = 98,hanmeimei = 99) # 根据关键字参数新建字典
print(d)

# d.items()以列表返回可遍历的(键, 值) 元组数组
d = {"a":1,"b":2,"c":4}
print(d.keys())# dict_keys(['a', 'b', 'c']) 返回列表
print(d.values())# dict_values([1, 2, 4]) 返回列表
print(d.items())# 返回的是一个列表，其中包括多个元组
# dict_items([('a', 1), ('b', 2), ('c', 4)])
print(list(d.items()))# 用list()转化为列表
# [('a', 1), ('b', 2), ('c', 4)]
for key, val in d.items():
    print("编号：{}，名称：{}".format(key, val))
# 编号：a，名称：1
# 编号：b，名称：2
# 编号：c，名称：4
```

### 字典与列表的互相嵌套

```python
# 在列表中存储字典
student1 = {"name":"lilei","age":18,"grade":98}
student2 = {"name":"hanmeimei","age":19,"grade":99}
student3 = {"name":"madongmei","age":18,"grade":95}

student = [student1,student2,student3]

print(student)
# 在字典中存储列表
favorite_class = {
    "lilei":["math","English"],
    "hanmeimei":["yuwen"],
    "madongmei":["computer","magic"]
}
print(favorite_class["lilei"])
print(favorite_class["madongmei"][0])
```

### 列表和字典的引用

```python
# Python中的赋值准确理解为贴标签，创建数据是相当于把数据放在盒子里，赋值时就是在盒子上贴标签
l1 = [1,2,3,4]
l2 = l1
l2[0] = 5
print(f'l2:{l2}')
print(f'l1:{l1}')# 修改l1时l2也发生了变化
#l2:[5, 2, 3, 4]
#l1:[5, 2, 3, 4]
# 用.copy()方法克隆一个数据
l1 = [1,2,3,4]
l2 = l1.copy()
l2[0] = 5
print(f'l1:{l1}')
print(f'l2:{l2}')# 修改l2时l1不发生变化
#l1:[1, 2, 3, 4]
#l2:[5, 2, 3, 4]
```

## 5.复数（complex）

`z = 1 + 2j`

print(z.real,z.imag) # 实部和虚部，都是浮点型

## 6.元组

列表和元组的区别：

- 列表元素用方括号包括，元组元素用圆括号包括；
- 列表中的元素可以修改、添加、删除，是可变的，而元组是不可变的。

元组创建后并非是不可改变的：

```python
l = [1,2,3]
t = (1,2,l)
l[0] = 5
print(t)
# (1, 2, [5, 2, 3])
# 元组中的可变元素是可以进行修改的
```

## 7.集合

集合中的元素是惟一的，不能重复的。

创建集合:

注意：创建一个空集合必须用 set() 而不是 { }，**因为 { } 是用来创建一个空字典**。

```python
set1 = {1,2,3,4,5,7,8}# 直接创建

set2 = set([1,2,3,4,1,2,4,6,6,4])# 用set()方法创建
print(set1,set2)
# {1, 2, 3, 4, 5, 7, 8} {1, 2, 3, 4, 6}
```

```python
# 1.集合的交集&
set1 = {1,2,3,4,5,8}
set2 = {1,2,3,5,9}
print(set1 & set2)
# {1, 2, 3, 5}

# 2.集合的并集|
set1 = {1,2,3,4,5,8}
set2 = {1,2,3,5,9}
print(set1 | set2)
# {1, 2, 3, 4, 5, 8, 9}

# 3.集合的差集，在set1中但是不在set2
set1 = {1,2,3,4,5,8}
set2 = {1,3,4,5,9}
print(set1 - set2)
# {8, 2}

# 4.集合的对称差集，在set1或set2中，不同时在两个集合中
set1 = {1,2,4,5,8}
set2 = {1,2,3,5,9}
print(set1 ^ set2)
#  {3, 4, 8, 9}
```

## 8.程序的控制结构

```python
# 1.if——else语句
if XXX:
    XXX
elif XXX:
    XXX
else:
    XXX
# 簡化： eg, 1 if 1 > 2 else 2
# 2.一般的while循环
while XXX:
    XXX
# 3.while——else循环
# 当while循环正常执行之后，继续执行else语句并结束
# 只有使用break跳出循环是才不会执行else语句
while XXX:
    XXX
else:
    XXX    
# example
i = 1
while i < 5:
    print(i)
    i += 1    
else:
    print("循环正常结束")
''' 输出如下：
1
2
3
4
循环正常结束
'''
# 4.break与continue
# 作用同C/C++中的break和continue，跳出循环，跳出本次循环
# 5.for循环
# for-in循环及range()的使用参见学习笔记一
# 6.for——else循环
# 只有使用break跳出循环是才不会执行else语句
for i in range(5):
    print(i)
else:
    print("循环正常结束")
''' 输出如下：
0
1
2
3
4
循环正常结束
'''    
```

## 9.异常处理

```python
# try-except结构
try:
    age = int(input("请输入年龄数字："))
    print(age)
except:
    print("输入有误！")
# try-except-else结构
try:
    age = int(input("请输入年龄数字："))
    print(age)
except:
    print("输入有误！")
else:
    print("输入正确！")# 当程序未出现异常（except）时，执行else语句
# try-except-else-finally结构
try:
    age = int(input("请输入年龄数字："))
    print(age)
except:
    print("输入有误！")
else:
    print("输入正确！")
finally:
    print("运行结束")# 不管程序是否出现异常都会执行finally语句
# 捕捉多种异常
try:
    age = int(input("请输入年龄数字："))
    print(age)
except ValueError:
    print("输入内容非数字！")
except:
    print("其他错误！")
```