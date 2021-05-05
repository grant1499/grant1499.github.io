---
title: C++ STL与Java util对比
tags:
  - STL
  - Java util
categories:
  - - C++
  - - Java
mathjax: true
copyright: true
abbrlink: 4c2b5dfd
date: 2021-04-04 21:55:49
---

## 常用容器操作

### 1.vector 和 ArrayList

<!--more-->

```C++
vector<int> a; - ArrayList<Integer> a = new ArrayList<>();
size() - size()  // 返回元素个数
empty() - isEmpty()  // 返回是否为空
clear() - clear  // 清空
front()/back() - get(0)/get(a.size() - 1)
push_back()/pop_back() - add()/remove(a.size() - 1)
begin()/end() - iterator()
[] - get()
支持比较运算 - 不支持比较运算
    
ArrayList 的遍历：
for (int i = 0; i < a.size(); i++) a.get(i);

Iterator<Integer> iterator = list.iterator();
while (iterator.hasNext()) iterator.next()

for (int num : a) num;
```

### 2.pair 和 Pair（Java 需要 import javafx.util.Pair ，目前 acwing 不支持）

```C++
pair<int, int> p = make_pair(1, 2); - Pair<Integer, Integer> p = new Pair<>(1, 2); 
first - getKey() // 第一个元素
second - getValue() // 第二个元素
支持比较运算 - 不支持比较运算
```

### 3.string 和 String

```C++
string a = "yxc"; - String b = "hqh";
size()/length() - length()  // 返回字符串长度
empty()
clear()
substr(起始下标，(子串长度)) - substring(起始下标，(终止下标 + 1)) // 返回子串
c_str()  // 返回字符串所在字符数组的起始地址
支持加法运算 - 支持加法运算
```

### 4.queue, stack, deque 和 Deque (有两个实现类，一般使用 ArrayDeque)

```C++
queue, 队列
   size() - size()
   empty() - isEmpty()
   push() - offer()  // 向队尾插入一个元素
   front() - peek()  // 返回队头元素
   back()  // 返回队尾元素
   pop() - poll()  // 弹出队头元素


stack, 栈
   size() - size()
   empty() - isEmpty()
   push() - push()  // 向栈顶插入一个元素
   top() - peek()  // 返回栈顶元素
   pop() - pop()  // 弹出栈顶元素

deque, 双端队列
   size() - size()
   empty() - isEmpty()
   clear() - clear()
   front()/back() - getFirst()/getLast()
   push_back()/pop_back() - offerLast()/pollLast()
   push_front()/pop_front() - offerFirst()/pollFirst()
   begin()/end() - iterator()
   []

作者：我要番茄锅
链接：https://www.acwing.com/blog/content/2180/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 5.priority_queue 和 PriorityQueue

```java
默认是大根堆 - 默认是小根堆
push() - offer()  // 插入一个元素
top() - peek()  // 返回堆顶元素
pop() - poll() // 弹出堆顶元素

PriorityQueue 定义成大根堆的方式：
PriorityQueue<Integer> pq = new PriorityQueue<>((o1, o2) -> o2 - o1);
```

### 6.unordered_set 和 HashSet

```C++
size() - size()
empty() - isEmpty()
clear() - clear()
insert() - add  // 插入一个数
find() - contains()  // 查找一个数
erase() - remove() // 删除元素
```

### 7.unordered_map 和 HashMap

```c++
size() - size()
empty() - isEmpty()
clear() - clear()
insert() - put() // 插入的数是一个 pair
find() - get(key) // 查找一个 pair
erase() - remove() // 删除元素
    - containsKey(key)/containsValue(value) // 判断元素是否在集合中
    - entrySet() // 返回一个包含所有节点的集合
    - keySet() // 返回一个包含所有键的集合
    - values() // 返回一个包含所有值的集合
    - getOrDefault(key, default value) // 返回指定 key 的 value，若 key 不存在 则返回 default value
    - putIfAbsent(key, value) // 如果集合中没有该 key 对应的节点，则插入
```

作者：我要番茄锅
链接：https://www.acwing.com/blog/content/2180/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。