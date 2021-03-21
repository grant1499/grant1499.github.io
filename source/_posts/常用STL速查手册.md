---
title: 常用STL速查手册
date: 2021-03-16 21:10:09
tags: [STL]
categories: 
	- [C++]
mathjax: true
copyright: true
---

作者：自豪的澡巾QAQ
链接：https://www.acwing.com/blog/content/6096/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

<!--more-->

这是我目前水平下能接触到的,如果有补充欢迎留言.
别抬杠,杠就是你对.

`#include<string>:`

常用操作:

```c++
string s;
s.begin();
s.end();
scanf("%s",s.c_str()); //使用scanf读入string类型的变量或者希望通过strcpy对字符串进行复制,请使用c_str();
s[i] == s.at(i);        //允许索引寻址
int indxe = s.find(char ch);    //返回ch在s中的索引
s.size();
s.append(string post);      
s.pop_back();
//返回s[begin,begin + len - 1],注意与java中的substring的区别,当len == -1时表示截取到最后
s.substr(int begin,int len);
insert(iterator iter,type val);
erase(iterator iter);
```

`#include<pair>`

常用操作

```c++
typedef pair<int,char> PII;//常结合map和sort使用
PII a;
a = make_pair(5,'f');//等价于a = {5,'f'};
a.first;a.second;//返回第一和第二关键字
```

`#include<vector>：`

vector的常用初始化操作:

```c++
//vector支持比较运算
vector<int> v;
vector<char> g(n,i);        //创建一个长度为n初值为i的一维向量(数组)
vector<char> f(g.begin() + [offset],g.end());       //通过g来创建一个f向量,offset可有可无
vector<int> c(v);        //将v复制给c
v.assign (7,100);             // 7 ints with a value of 100
```

vector的常用函数:

```c++
clear();
begin();
end();
size();
empty();
front();
back();
push_back(member_type x);
pop_back();
erase(iterator l[,iterator r]);        // 删除第l[到r]个值
v.at[i] == v[i]
```

`#include<stack>:`

常用操作:

```c++
push(type x);   //压栈
pop()   //出栈
empty();    
size();
top();  //返回栈顶
```

`#include<queue>`

常用操作:

```c++
/*
queue<int> q;
*/
q.push(int x);   //入队
q.pop();  //出队
q.empty();
q.size();
q.front();    //队首

q.back(); //队尾
-------------------
/*
priority_queue<int> heap;   //优先队列,即堆,默认为大根堆
定义小根堆:
    priority_queue<int,vector<int>,greater<int>> tt;
*/
heap.size();
heap.empty();
heap.push(int x);
heap.pop();     //移除堆顶元素
heap.top();     //返回堆顶元素
```

`#include<deque>`

常用操作

```c++
/*
deque<int> q;//支持随机寻址,效率相较低
*/
q.empty();
q.size();
q.front();
q.back();
q.push_back(int x);
q.push_front(int x);
q.pop_back();
q.pop_front();
```

`#include<map>`

常用操作

```c++
/*
//存储的类型为pair，因此可以通过类似dic.begin().first(second)来访问键值对
map<string,int> dic;//不可以有重复的键值,multimap可以有重复的键值
*/
dic.empty();
dic.size();
dic.insert({"hello",1});    //参数类型为pair,等价于dic["hello"] = 1;
dic.erase({"hello",1});     //参数类型为pair或者iterator
dic.find("hello");          //注意返回的是迭代器
dic.count("hello");         //返回0或者1
```

`#include<set>`

常用操作

```c++
/*
set的含义是集合,元素有序,支持插入,删除,查找等操作,所有的操作的都是严格在logn时间之内完成,效率非常高。
set和multiset的区别是：set插入的元素不能相同，但是multiset可以相同。
*/
set/multiset<int> s;
s.insert(int x);
s.find(int x);  //返回迭代器
s.erase(int x/iterator);//如果参数为整数,意为删除集合内所有x;如果是迭代器,则删除这个迭代器
```

`#include<unordered_set/multiset/unordered_map/multimap>`

常用操作
带有unordered意味着底层为hash,与上面的操作类似,但时间复杂度为O(1)
`#include<algorithm>`

常用函数
**最大值,最小值,交换**

```c++
max(max(a,b),c);
min();
swap(a,b);
```

**快速排序,默认排序结果为升序;**

```c++
int num[] = {5,6,9,10,6,2};
sort(num,num + 6,cmp);//cmp可省略,可以手动实现cmp定义排序规则,通常与struct、pair形成combo
```

**字符串反转**

```c++
string s = "hello wolrd";
reverse(s.begin(),s.end());//能够将string,vector等类型进行翻转,返回迭代器
cout << s << endl;  //    "dlrow olleh";
```

**去重**

```c++
int num[] = {10,20,20,20,30,30,20,20,10};
vector<int> v(num,num + 9);
sort(v.begin(),v.end());
/*
删除重复出现的系列点,unique()函数将所有不重复的点放在数组前面,重复的点放在后面,
返回第一个起始重复元素的迭代器
*/
v.erase(unique(v.begin(),v.end()),v.end());
for (auto a : v) cout << a << ' ';      //10 20 30
```

**全排列**

```c++
/*
next_permutation()将按字母表顺序生成给定序列的下一个较大的排列,直到整个序列为降序为止。
prev_permutation()与之相反,是生成给定序列的上一个较小的排列。
*/
int num[] = {1,2,3,4,5};
int cnt = 1;
while (next_permutation(num,num + 5)) {
    cnt++;
    for (auto x : num) cout << x << ' ';
    cout << endl;
}
cout << cnt;// cnt == 5! == 120
```

**本内容仅用作STL简单使用字典**