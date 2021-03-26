---
title: C++竞赛语法总结（八）
date: 2021-02-24 07:03:17
tags: [竞赛语法,STL,位运算,库函数]
categories: 
	- [C++]
	- [算法]
mathjax: true
copyright: true
---

(续上文)

------

### 3.5 `#include <set> `

头文件set主要包括set和multiset两个容器，分别是“有序集合”和“有序多重集合”，**即前者的元素不能重复，而后者可以包含若干个相等的元素**。set和multiset的内部实现是一棵红黑树，它们支持的函数基本相同。 

<!--more-->

#### 3.5.1 声明

```C++
    set<int> s;

struct rec{…}; set<rec> s;   // 结构体rec中必须定义小于号

multiset<double> s;
```

#### 3.5.2 size/empty/clear

​    与vector类似 

#### 3.5.3 迭代器

set和multiset的迭代器称为“双向访问迭代器”，不**支持“随机访问”，支持星号(*)解除引用，仅支持”++”和–“两个与算术相关的操作**。

设it是一个迭代器，例如`set<int>::iterator it;`

若把`it++`，则it会指向“下一个”元素。这里的“下一个”元素是指在元素从小到大排序的结果中，排在it下一名的元素。同理，若把`it--`，则it将会指向排在“上一个”的元素。

#### 3.5.4 begin/end

- 返回集合的首、尾迭代器，时间复杂度均为O(1)。
- s.begin() 是指向集合中最小元素的迭代器。
- s.end() 是指向集合中最大元素的下一个位置的迭代器。**换言之，就像vector一样，是一个“前闭后开”的形式**。**因此–s.end()是指向集合中最大元素的迭代器**。

#### 3.5.6 insert

- s.insert(x)把一个元素x插入到集合s中，时间复杂度为O($logn$)。
- 在set中，若元素已存在，则不会重复插入该元素，对集合的状态无影响。

#### 3.5.7 find

s.find(x) 在集合s中查找等于x的元素，并返回指向该元素的迭代器。若不存在，则返回s.end()。时间复杂度为O($logn$)。

#### 3.5.8 lower_bound/upper_bound

- 这两个函数的用法与find类似，但查找的条件略有不同，时间复杂度为 O($logn$)。
- s.lower_bound(x) 查找大于等于x的元素中最小的一个，并返回指向该元素的迭代器。
- s.upper_bound(x) 查找大于x的元素中最小的一个，并返回指向该元素的迭代器。

#### 3.5.9 erase

- 设it是一个迭代器，s.erase(it) 从s中删除迭代器it指向的元素，时间复杂度为O$logn$)
- 设x是一个元素，s.erase(x) 从s中删除所有等于x的元素，时间复杂度为O(k+$logn$)，其中k是被删除的元素个数。 

#### 3.5.10 count

​       s.count(x) 返回集合s中等于x的元素个数，时间复杂度为 O(k +$logn$)，其中k为元素x的个数。

#### 3.5.11 实例

```C++
#include <iostream>
#include <set>
using namespace std;
int main()
{
    set<int> a;// 不能包含重复元素，否则会忽略操作
    multiset<int> b;// 可以包含重复元素
    // set中需要作比较，所以必须重载
    struct Rec
    {
        int x,y;
        bool operator< (const Rec& t) const
        {
            return x < t.x;
        }
    };
    set<Rec> c;

    set<int>::iterator it = a.begin();
    it ++, it --;
    ++ it, -- it;
    a.end();

    a.insert(x);
    if (a.find(x) == a.end()) // 判断x在a中是否存在
    a.lower_bound(x);// 找到大于等于x的最小元素的迭代器
    a.upper_bound(x);// 找到大于x的最小元素的迭代器，很容易犯错
    a.count(x);
    return 0;
}
```

### 3.6 `#include <map>`

map容器是一个键值对key-value的映射，其内部实现是一棵以key为关键码的红黑树。Map的key和value可以是任意类型，其中key必须定义小于号运算符。

#### 3.6.1 声明

​       `map<key_type, value_type> name;`

​       例如：

​       `map<long, long, bool> vis;`

​       `map<string, int> hash;`

​       `map<pair<int, int>, vector<int>> test;`

#### 3.6.2

​    size/empty/clear/begin/end均与set类似。 

#### 3.6.3 Insert/erase

​       与set类似，但其参数均是`pair<key_type, value_type>`。

#### 3.6.4 find

​       h.find(x) 在变量名为h的map中查找key为x的二元组。

#### 3.6.5  []操作符

​       h[key] 返回key映射的value的引用，时间复杂度为O(logn)。

[]操作符是map最吸引人的地方。我们可以很方便地通过h[key]来得到key对应的value，还可以对h[key]进行赋值操作，改变key对应的value。

#### 3.6.6 实例

```C++
#include <iostream>
#include <map>
#include <vector>
using namespace std;
int main()
{
    map<int,int> a;
    a[1] = 1;
    a[10000] = 3;
    cout << a[10000] << endl;
    map<string,vector<int>> b,c;
    b["yxc"] = vector<int>();
    c["llx"] = vector<int>({1,2,3,4});
    cout << b["yxc"].size() << endl;
    cout << c["llx"][2] << endl;
    b.insert({"as",{}});

    string str = "as";
    cout << (b.find(str) == b.end()) << endl;
    
    return 0;
}
```

### 3.7 补充的几个STL

```C++
#include <iostream>
#include <unordered_set>
#include <unordered_map>
#include <bitset>
using namespace std;
int main()
{
    unordered_set<int> S;// 底层由哈希表实现，不能存储重复元素
    /* 与set的区别：
    1.比set少了lower_bound/upper_bound，因为是无序的
    2.主要操作复杂度是O(1)，而set是O(logn)，相对效率更高
    */
    unordered_multiset<int> b;// 可以存储重复元素

    unordered_map<int,int> c;// 哈希表，与map几乎完全一样
    // 效率更高，都是O(1)，而map是O(logn)，但不支持二分

    bitset<1000> a,t;
    a[0] = 1;
    a[1] = 1;
    a.set(3);// 把a[3]设为1
    cout << a[3] << endl;
    a.reset(3);// 把a[3]设为0
    cout << a[3] << endl;
    return 0;
}
```

### 3.8 二元组(pair)

```C++
#include <iostream>
#include <vector>
using namespace std;
int main()
{
    pair<int,string> a,b;
    a = {3,"yxc"};// C++99不支持这种赋值
    a = make_pair(4,"abc");
    cout << a.first << ' ' << a.second << endl;
    // pair支持a >= b,a <= b,a == b,a != b等比较运算
    // 先比较first，再比较second
    vector<int> c,d;
    // if(c == d)，vector也支持按字典序进行比较，先比较数组第一个位置
    return 0;
}
```

## 4.位运算与常见库函数

C++帮我们实现好了很多有用的函数，我们要避免重复造轮子。                                                               ——yxc

### 4.1 位运算

其实是数学上的概念。

| 符号  | 运算                    |
| ----- | ----------------------- |
| &     | 与(AND)                 |
| \|    | 或(OR)                  |
| **~** | 非(NOT)                 |
| ^     | 异或(XOR)（不进位的加） |
| `>>`  | 右移                    |
| `<<`  | 左移                    |

```C++
0&0 = 0, 0&1 = 0, 1&0 = 0, 1&1 = 1
0|0 = 0, 1|0 = 1, 0|1 = 1, 1|1 = 1
~0 = 1, ~1 = 0
0^0 = 0, 1^1 = 0, 1^0 = 1, 0^1 = 1 
```

总之,在C中,左移是逻辑/算术左移(两者完全相同),右移是算术右移,会保持符号位不变.实际应用中可以根据情况用左/右移做快速的乘/除运算,这样会比循环效率高很多.

**常用操作：**

(1)  **求x的第k位数字 x >> k & 1**

(2)  **lowbit(x) = x & -x=x & (~x + 1)，返回x的最后一位1及以后的数**

比如12（10）二进制表示为 1100（2），`lowbit(12) == 100（2）== 4（10）`。

(3) 我们可以直观的发现，如果是一个偶数^1,那么答案是偶数+1.如果是一个奇数^1,那么答案是奇数-1。

```C++
#include <iostream>
using namespace std;
int main()
{
    int a = 3,b = 6;
    cout << (a^b) << endl;
    a = 13;
    cout << (a >> 2 & 1) << endl;
    for (int i = 5;i >= 0;i --) cout << (a >> i & 1);
    cout << endl;
    int c = 2345256;
    int d = -c;
    int e = ~c + 1;cout << d << ' ' << e << endl;
    return 0;
}
```

## 5.acwing.67.数字在排序数组中出现的次数

题目描述
求一个排好序的数组中k的个数。例如输入排序数组[1, 2, 3, 3, 3, 3, 4, 5]和数字3，由于3在这个数组中出现了4次，因此输出4。

解题思路
目的是练习stl和常用的库函数

### 题解一：

使用有序多重集合multiset

```C++
class Solution {
public:
    int getNumberOfK(vector<int>& nums , int k) {
        multiset<int> s;

        for(int x : nums) s.insert(x);

        return s.count(k);
    }
};
```

### 题解二：

遍历vector，计数

```C++
class Solution {
public:
    int getNumberOfK(vector<int>& nums , int k) {
        int cnt = 0;
        for(int x : nums)
            if(x == k)
                cnt++;
        return cnt;
    }
};
```

### 题解三：

二分做法，使用lower_bound和upper_bound,指针运算得出次数

```C++
class Solution {
public:
    int getNumberOfK(vector<int>& nums , int k) {

        auto l = lower_bound(nums.begin(), nums.end(), k);//第一个大于等于x的数的地址
        auto r = upper_bound(nums.begin(), nums.end(), k);//第一个大于x的数的地址

        return r - l;
    }
};
```

作者：醉生梦死
链接：https://www.acwing.com/solution/content/16094/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

### 关于vector中的lower_bound和upper_bound

#### 共同点

比较的“首”地址+ 一个数组元素的地址（对应的这个数组里边任意一个元素的地址，表示这个二分里边的比较的”结尾’地址）+ 你要二分查找的那个数。

一个数组元素的地址（或者数组名来表示这个数组的首地址，用来表示这个数组的开头比较的元素的地址，不一定要是首地址，只是用于比较的“首”地址）**+** 一个数组元素的地址（对应的这个数组里边任意一个元素的地址，表示这个二分里边的比较的”结尾’地址）**+** 你要二分查找的那个数。

例如：

```C++
lower_bound(r[x].begin(),r[x].end(),l)
upper_bound(r[x].begin(),r[x].end(),R)
```

#### 区别

**lower_bound**

**返回第一个大于等于x的数的地址**

**upper_bound**

**返回第一个大于x的数的地址**

## 6.acwing.68.数组中唯一只出现一次的数字

```C++
// solution 1，我的题解
class Solution {
public:
    int getMissingNumber(vector<int>& nums) {
        int i;
        for (i = 0;i < nums.size();i ++)
        {
            if  (nums[i] != i) break;
        }
        return i;
    }
};
// solution 2，y总题解，利用哈希表
class Solution {
public:
    int getMissingNumber(vector<int>& nums) {
        unordered_set<int> S;
        for (int i=0;i <= nums.size();i ++) S.insert(i);
        for (int x :nums) S.erase(x);
        return *S.begin();
    }
};
```

## 7.acwing.32. 调整数组顺序使奇数位于偶数前面

主要是不会交换vector元素的位置。思路还是知道的（双指针）。不要用迭代器做！

输入一个整数数组，实现一个函数来调整该数组中数字的顺序。

使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分。

样例

```C++
输入：[1,2,3,4,5]

输出: [1,3,5,2,4]
```

**算法**
**(双指针扫描) O(n)**
用两个指针分别从首尾开始，往中间扫描。扫描时保证第一个指针前面的数都是奇数，第二个指针后面的数都是偶数。

每次迭代时需要进行的操作：

第一个指针一直往后走，直到遇到第一个偶数为止；
第二个指针一直往前走，直到遇到第一个奇数为止；
交换两个指针指向的位置上的数，再进入下一层迭代，直到两个指针相遇为止；
**时间复杂度**
当两个指针相遇时，走过的总路程长度是 n，所以时间复杂度是 $O(n)$。

```C++
class Solution {
public:
    void reOrderArray(vector<int> &array) {
         int l = 0, r = array.size() - 1;
         while (l < r) {
             while (l < r && array[l] % 2 == 1) l ++ ;
             while (l < r && array[r] % 2 == 0) r -- ;
             if (l < r) swap(array[l], array[r]);
         }
    }
};
```

## 8.acwing.17.从尾到头打印链表

输入一个链表的头结点，按照 **从尾到头** 的顺序返回节点的值。

返回的结果用数组存储。

```C++
// solution 1，我的题解
class Solution {
public:
    vector<int> printListReversingly(ListNode* head) {
        vector<int> a;
        for (ListNode* p = head;p != NULL;p = p->next) a.push_back(p->val);
        int j = 0,k = a.size() - 1;
        while (j < k)
        {
            swap(a[j],a[k]);
            j++,k--;
        }
        return a;
    }
};
// solution 2,递归 vector 打印
class Solution {
public:
    vector<int> printListReversingly(ListNode* head) {
        vector<int> vi;
        if(head == NULL) 
            return vi;
        else
        {
            vi =printListReversingly(head->next);
            vi.push_back(head->val);
            return vi;
        }

    }
};
作者：雪山肥鱼
链接：https://www.acwing.com/solution/content/9900/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
// solution 3,y总题解
class Solution {
public:
    vector<int> printListReversingly(ListNode* head) {
        vector<int> temp;
        while(head!=NULL)
        {
            temp.push_back(head->val);
            head=head->next;
        }
        reverse(temp.begin(),temp.end());// reverse vector
        return temp;
    }
};
```