---
title: C++竞赛语法总结（九）
tags:
  - 竞赛语法
  - 库函数
  - STL
  - 位运算
  - 栈实现队列
categories:
  - - C++
  - - 算法
mathjax: true
copyright: true
abbrlink: cc84a5fd
date: 2021-02-24 07:03:28
---

## 1.常用库函数

这些库函数大都在`<algorithm>`中。

<!--more-->

### (1)  reverse 翻转

既可以用于一般的数组，也能够用于vector。

翻转一个vector：

```C++
reverse(a.begin(), a.end());
```

翻转一个数组，元素存放在下标1~n：

```C++
reverse(a + 1, a + 1 + n);
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
int main()
{
    vector<int> a({1,2,3,4,5});
    reverse(a.begin(), a.end());// 左闭右开
    for (int x:a) cout << x << ' ';
    cout << endl;

    int b[] = {1,2,3,4,5};
    reverse(b,b + 5);// 左闭右开
    for (int x:a) cout << x << ' ';
    cout << endl;
    return 0;
}
```

### (2)  unique 去重

去除容器中相邻的重复元素。(**必须保证重复元素是相邻排列的**)

返回去重之后的尾迭代器（或指针），仍然为前闭后开，即这个迭代器是去重之后末尾元素的下一个位置。该函数常用于离散化，**利用迭代器（或指针）的减法，可计算出去重后的元素个数**。

注：数组去重之后，**去重之后的多余元素并没有删除，而是放在后面**。

把一个vector去重：

```C++
int m = unique(a.begin(), a.end()) – a.begin();
```

把一个数组去重，元素存放在下标1~n：

```C++
int m = unique(a + 1, a + 1 + n) – (a + 1);
```

**补充`vector::erase`：（左闭右开）**

从指定容器删除指定位置的元素或某段范围内的元素，vector::erase()方法有两种重载形式。

```C++
如下： iterator erase(  iterator _Where); iterator erase(  iterator _First,  iterator _Last); 
```

  如果是删除指定位置的元素时：返回值是一个迭代器，指向删除元素下一个元素 ；

 如果是删除某范围内的元素时：返回值也表示一个迭代器，指向最后一个删除元素的下一个元素。

```C++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
int main()
{
    int a[] = {1,1,2,2,3,3,4};
    int m = unique(a,a + 7) - a;
    cout << m << endl;
    for (int i = 0;i < m;i ++) cout << a[i] << ' ';
    cout << endl;
    
    for (int i = 0;i < 7;i ++) cout << a[i] << ' ';
    cout << endl;// 去重之后的多余元素并没有删除

    vector<int> b({1,1,2,2,3,3,4});
    int n = unique(b.begin(),b.end()) - b.begin();
    cout << n << endl;
    for (int i = 0;i < n;i ++) cout << a[i] << ' ';
    cout << endl;

    vector<int> c({1,1,2,2,3,3,4});
    c.erase(unique(c.begin(),c.end()),c.end());// 常见的用法
    for (auto x: c) cout << x << ' ';
    cout << endl;
    return 0;
}
```

### (3)  random_shuffle 随机打乱

用法与reverse相同 ,参数与reverse一样。生成随机数排列。

```C++
#include <iostream>
#include <algorithm>
#include <vector>
#include <ctime>
using namespace std;
int main()
{
    vector<int> a({1,2,3,4,5});
    srand(time(0));
    random_shuffle(a.begin(),a.end());
    for (int x:a) cout << x << ' ';
    cout << endl;
    return 0;
}
```

### (4)  sort(非常非常重要)

前两个参数与reverse完全一样。

对两个迭代器（或指针）指定的部分进行快速排序。可以在第三个参数传入定义大小比较的函数，或者重载“小于号”运算符。

把一个int数组（元素存放在下标1~n）从大到小排序，传入比较函数：

 `int a[MAX_SIZE];`

`bool cmp(int a, int b) {return a > b;}`

`sort(a + 1, a + 1 + n, cmp);`

```C++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
bool cmp(int a,int b)// a是否应该排在b的前面
{
    return a > b;// 从大到小
}
int main()
{
    vector<int> a({1,2,3,4,5});
    sort(a.begin(),a.end());// 从小到大
    for (int x :a) cout << x << ' ';
    cout << endl;

    sort(a.begin(),a.end(),greater<int>());// 从大到小
    for (int x :a) cout << x << ' ';
    cout << endl;

    sort(a.begin(),a.end(),cmp);// 根据cmp排序
    for (int x :a) cout << x << ' ';
    cout << endl;
    return 0;
}
```

#### 结构体排序

solution 1:自定义比较函数cmp

```C++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
struct Rec{
        int x,y;
    }a[5];
bool cmp(Rec a,Rec b)// a是否应该排在b的前面
{
    return a.x < b.x;// 从小到大
}
int main()
{
    for (int i = 0;i < 5;i ++)
    {
        a[i].x =-i;
        a[i].y = i;
    }
    for (int i = 0;i < 5;i ++) printf("(%d,%d) ",a[i].x,a[i].y);
    cout << endl;

    sort(a,a + 5,cmp);
    for (int i = 0;i < 5;i ++) printf("(%d,%d) ",a[i].x,a[i].y);
    cout << endl;
    return 0;
}
```

solution 2:把自定义的结构体vector排序，重载“小于号”运算符：

```C++
struct rec{ int id, x, y; }

vector<rec> a;

bool operator <(const rec &a, const rec &b) {

	return a.x < b.x || a.x == b.x && a.y < b.y;

}

sort(a.begin(), a.end());
----------------------------------------------------------------------------------
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
struct Rec{
        int x,y;
        bool operator< (const Rec&t) const
        {
            return x < t.x;// 从小到大排序
        }
    }a[5];
int main()
{
    for (int i = 0;i < 5;i ++)
    {
        a[i].x =-i;
        a[i].y = i;
    }
    for (int i = 0;i < 5;i ++) printf("(%d,%d) ",a[i].x,a[i].y);
    cout << endl;

    sort(a,a + 5);
    for (int i = 0;i < 5;i ++) printf("(%d,%d) ",a[i].x,a[i].y);
    cout << endl;
    return 0;
}
```

### (5)  lower_bound/upper_bound 二分

二分的前提是**数组或者结构体已经从小到排好序了**。

lower_bound 的第三个参数传入一个元素x，在两个迭代器（指针）指定的部分上执行二分查找，返回指向第一个**大于等于x的元素的位置**的迭代器（指针）。

upper_bound 的用法和lower_bound大致相同，唯一的区别是**查找第一个大于x的元素**。当然，两个迭代器（指针）指定的部分应该是提前排好序的。

在有序int数组（元素存放在下标1~n）中查找大于等于x的最小整数的下标：

```C++
// 写法1
int* l = lower_bound(a + 1, a + 1 + n, x);
int t = l - a;
// 写法2
int l = lower_bound(a + 1, a + 1 + n, x) - a;
```

在有序`vector<int>` 中查找小于等于x的最大整数（假设一定存在）：

`int y = *--upper_bound(a.begin(), a.end(), x);`

```C++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
int main()
{
    int a[] = {1,2,4,5,6};
    int* p = lower_bound(a,a + 5,3);
    int* q = lower_bound(a,a + 5,4);
    int t1 = lower_bound(a,a + 5,6) - a;// 返回第一个>=6的元素的下标
    int t2 = lower_bound(a,a + 5,7) - a;
    cout << t1 << ' ' << t2 << endl;
    cout << *p << ' ' << *q <<  endl;

    vector<int> b{1,2,3,4,5,6,7};// 去掉()也能初始化vector
    int t = upper_bound(b.begin(),b.end(),4) - b.begin();
    cout << t << ' ' << b[t] << endl;
    return 0;
}
```

### (6) next_permutation()（全排列函数）

将当前排列更改为 **全排列中的下一个排列**。如果当前排列已经是 **全排列中的最后一个排列**（元素完全从大到小排列），函数返回 `false` 并将排列更改为 **全排列中的第一个排列**（元素完全从小到大排列）；否则，函数返回 `true`。`next_permutation(v.begin(), v.end())` 或 `next_permutation(v + begin, v + end)`。

```c++
/*
next_permutation()将按字母表顺序生成给定序列的下一个较大的排列,直到整个序列为降序为止。
prev_permutation()与之相反,是生成给定序列的上一个较小的排列。
*/
#include <iostream>
#include <algorithm>
using namespace std;
int main()
{
	int num[] = {1,2,3,4,5};
	int cnt = 1;
	while (next_permutation(num,num + 5)) {
		cnt++;
		for (auto x : num) cout << x << ' ';
		cout << endl;
	}
	cout << cnt;// cnt == 5! == 120
	return 0;
}
/*1 2 3 5 4 
1 2 4 3 5
...
5 4 3 2 1
120*/
```

## 2.acwing.53.最小的k个数

输入n个整数，找出其中最小的k个数。

**注意：**

- 数据保证k一定小于等于输入数组的长度;
- 输出数组内元素请按从小到大顺序排序;

样例

```C++
输入：[1,2,3,4,5,6,7,8] , k=4

输出：[1,2,3,4]
```

```C++
// solution 1,我的题解，利用优先队列（堆）
class Solution {
public:
    vector<int> getLeastNumbers_Solution(vector<int> input, int k) {
        priority_queue<int,vector<int>,greater<int>> p;// 小根堆
        vector<int> res;
        for (int i = 0;i < input.size();i ++) p.push(input[i]);
        for (int i = 0;i < k;i ++) {res.push_back(p.top());p.pop();}
        return res;
    }
};
// solution 2,y总题解，练习sort
class Solution {
public:
    vector<int> getLeastNumbers_Solution(vector<int> input, int k) {
        vector<int> res;
        sort(input.begin(),input.end());
        for (int i = 0;i < k;i ++) res.push_back(input[i]);
        return res;
    }
};
```

## 3.acwing.75.和为S的两个数字

输入一个数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。

如果有多对数字的和等于s，输出任意一对即可。

你可以认为每组输入中都至少含有一组满足条件的输出。

样例

```C++
输入：[1,2,3,4] , sum=7

输出：[3,4];
```

```C++
// solution 1，暴力枚举
//两个遍历数组的循环，所以时间复杂度是O(n^2)
//没有开辟与数组大小相关的空间，所以空间复杂度是O(1)
class Solution {
public:
    vector<int> findNumbersWithSum(vector<int>& nums, int target) {
        vector<int> res;
        for (int i = 0;i < nums.size();i ++)
        {
            for (int j = i+1;j < nums.size();j ++)
            {
                if (nums[i] + nums[j] == target)
                {
                    res.push_back(nums[i]);
                    res.push_back(nums[j]);
                    return res;
                }
            }
        }
    }
};
// solution 2，y总题解，哈希表
//时间复杂度O(n)；开辟了数组大小的哈希表，所以空间复杂度是O(n)
class Solution {
public:
    vector<int> findNumbersWithSum(vector<int>& nums, int target) {
        unordered_set<int> S;
        for (int x:nums)
        {
            if(S.count(target-x)) return {x,target-x};
            S.insert(x);
        }
    }
};
```

## 4.acwing.51.数字排列

有难度！！不会了！

输入一组数字（可能包含重复数字），输出其所有的排列方式。

样例

```C++
输入：[1,2,3]

输出：
      [
        [1,2,3],
        [1,3,2],
        [2,1,3],
        [2,3,1],
        [3,1,2],
        [3,2,1]
      ]
```

**`next_permutation`全排列函数详解：**

- `next_permutation`：将当前排列更改为 **全排列中的下一个排列**。如果当前排列已经是 **全排列中的最后一个排列**（元素完全从大到小排列），函数返回 `false` 并将排列更改为 **全排列中的第一个排列**（元素完全从小到大排列）；否则，函数返回 `true`。`next_permutation(v.begin(), v.end())` 或 `next_permutation(v + begin, v + end)`。
- 使用 `next_permutation` 生成 到 的全排列。例题：[Luogu P1706 全排列问题](https://www.luogu.com.cn/problem/P1706)

```C++
int N = 9, a[] = {1, 2, 3, 4, 5, 6, 7, 8, 9};
do {
  for (int i = 0; i < N; i++) cout << a[i] << " ";
  cout << endl;
} while (next_permutation(a, a + N));
```

**题解：**

```C++
// solution 1,STLnb!!!
class Solution {
public:
    vector<vector<int>> permutation(vector<int>& nums) {
        vector<vector<int>> res;
        do res.push_back(nums);while (next_permutation(nums.begin(),nums.end()));
        return res;
    }
};
```

## 5.acwing.26.二进制中1的个数

输入一个32位整数，输出该数二进制表示中1的个数。

```C++
// solution 1，我的题解
class Solution {
public:
    int NumberOf1(int n) {
        int res = 0;
        for (int i = 31;i >= 0;i --)
        {
            if ((n >> i & 1) == 1) res++;
        }
        return res;
    }
};
// solution 2，y总题解，lowbit写法，更快一点
//lowbit(x) = x & -x，取二进制最后一位1及以后的数
class Solution {
public:
    int NumberOf1(int n) {
        int res = 0;
        while (n) {n -= n & -n; res++;}
        return res;
    }
};
```

## 6.acwing.862.三元组排序

```C++
// solution 1，我的题解，结构体普通做法
#include <iostream>
#include <algorithm>
using namespace std;
struct Rec
{
	int a;
    float b;
    string c;
}R[10000];
bool cmp(Rec x,Rec y)
{
	return x.a < y.a;
}
int main()
{
	int N;
	cin >> N;
	int i = 0;
	while (N--)
	{
		cin >> R[i].a >> R[i].b >> R[i].c;
		i ++;
	}
	sort(R,R+i,cmp);
	for (int j = 0;j < i;j ++)
	{
	    printf("%d %.2f %s\n",R[j].a,R[j].b,R[j].c.c_str());
        // 注意printf输出string得用string.c_str()
	}
	return 0;
}
// solution 2
//STL:vector 这里用了pair<int, pair<double, string >> 嵌套pair构成三元组
#include<iostream>
#include<cstring>
#include<algorithm>
#include<vector>
#define x first
#define y second
using namespace std;

const int N = 10010;

typedef pair<int, pair<double, string >> PII;

vector<PII> ans;
int n, a;
double b;
string s;
int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ )
    {
        cin >> a >> b >> s;
        ans.push_back({a, {b, s}});
    }

    sort(ans.begin(), ans.end());

    for (auto i: ans)
        printf("%d %.2lf %s\n",i.x, i.y.x, i.y.y.c_str());   
    return 0;
}
作者：小张同学
链接：https://www.acwing.com/solution/content/9475/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
// solution 3,STL : map
#include<iostream>
#include<cstring>
#include<cstdio>
#include<map>
#define x first
#define y second
using namespace std;

const int N = 10010;

typedef pair<double, string> PII;
map<int, PII> ans;
int n;

int main()
{
    int a;
    double b;
    string c;
    cin >> n;
    for (int i = 0; i < n; i ++ )
    {
        cin >> a >> b >> c;
        ans.insert({a, {b, c}});
    }
    // map<int, PII>::iterator iter; 迭代器
    for (map<int, PII>::iterator iter = ans.begin(); iter != ans.end(); iter ++ )
        printf("%d %.2f %s\n", iter->x, iter->y.x, iter->y.y.c_str());  //这里 iter -> x/y 是map ，后面两个是pair ：PII.first/second
    return 0;
}
作者：小张同学
链接：https://www.acwing.com/solution/content/9475/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## 7.acwing.20.用两个栈实现队列（剑指offer）

请用栈实现一个队列，支持如下四种操作：

- push(x) – 将元素x插到队尾；
- pop() – 将队首的元素弹出，并返回该元素；
- peek() – 返回队首元素；
- empty() – 返回队列是否为空；

**注意：**

- 你只能使用栈的标准操作：`push to top`，`peek/pop from top`, `size` 和 `is empty`；
- 如果你选择的编程语言没有栈的标准库，你可以使用list或者deque等模拟栈的操作；
- 输入数据保证合法，例如，在队列为空时，不会进行`pop`或者`peek`等操作；

样例

```C++
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);
queue.peek();  // returns 1
queue.pop();   // returns 1
queue.empty(); // returns false
```

**算法：**(栈，队列) O(n)
这是一道基础题，只要把功能实现对就可以，不需要考虑运行效率。

**我们用两个栈来做，一个主栈，用来存储数据；一个辅助栈，用来当缓存**。

- `push(x)`，我们直接将x插入主栈中即可。
- `pop()`，此时我们需要弹出最先进入栈的元素，也就是栈底元素。我们可以先将所有元素从主栈中弹出，压入辅助栈中。则辅助栈的栈顶元素就是我们要弹出的元素，将其弹出即可。然后再将辅助栈中的元素全部弹出，压入主栈中。
- `peek()`，可以用和pop()操作类似的方式，得到最先压入栈的元素。
- `empty()`，直接判断主栈是否为空即可。
  时间复杂度分析
- `push()`：O(1)；
- `pop()`: 每次需要将主栈元素全部弹出，再压入，所以需要 O(n)的时间；
- `peek()`：类似于pop()，需要 O(n) 的时间；
- `empty()`：O(1)；

作者：yxc
链接：https://www.acwing.com/solution/content/725/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```C++
class MyQueue {
public:
    stack<int> s1,s2;
    /** Initialize your data structure here. */
    MyQueue() {
        
    }    
    /** Push element x to the back of queue. */
    void push(int x) {
        s1.push(x);
    }    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        while (s1.size() > 1) s2.push(s1.top()),s1.pop();// 注意这里是逗号
        int t = s1.top();
        s1.pop();
        while (s2.size()) s1.push(s2.top()),s2.pop();
        return t;
    }    
    /** Get the front element. */
    int peek() {
        while (s1.size() > 1) s2.push(s1.top()),s1.pop();
        int t = s1.top();
        while (s2.size()) s1.push(s2.top()),s2.pop();
        return t;
    }    
    /** Returns whether the queue is empty. */
    bool empty() {
        return s1.empty();
    }
};
```

# Acwing：语法基础课系列正式完结！！！