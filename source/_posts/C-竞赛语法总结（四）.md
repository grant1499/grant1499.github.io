---
title: C++竞赛语法总结（四）
tags:
  - 竞赛语法
categories:
  - - C++
  - - 算法
mathjax: true
copyright: true
abbrlink: 99b255e0
date: 2021-02-24 07:02:37
---

#### 1.acwing768.忽略大小写比较字符串大小

一般我们用strcmp可比较两个字符串的大小，比较方法为对两个字符串从前往后逐个字符相比较（按ASCII码值大小比较），直到出现不同的字符或遇到'\0'为止。

<!--more-->

如果全部字符都相同，则认为相同；如果出现不相同的字符，则以第一个不相同的字符的比较结果为准。

但在有些时候，我们比较字符串的大小时，希望忽略字母的大小，例如"Hello"和"hello"在忽略字母大小写时是相等的。

请写一个程序，实现对两个字符串进行忽略字母大小写的大小比较。

 输入格式

输入为两行，每行一个字符串，共两个字符串。注意字符串中可能包含空格。

数据保证每个字符串的长度都不超过80。

 输出格式

如果第一个字符串比第二个字符串小，输出一个字符"<"。

如果第一个字符串比第二个字符串大，输出一个字符">"。

如果两个字符串相等，输出一个字符"="。

```C++
// 自己写的题解
#include <iostream>
#include <string>

using namespace std;

int main()
{
    string a,b;
    getline(cin,a);
    getline(cin,b);
    cin >> a >> b;
    char op[3] = {'<','>','='};
    int k = -1;
    for (int i = 0;a[i] && b[i];i++)
    {
        a[i] = a[i] < 'a'? a[i] + 32:a[i];
        b[i] = b[i] < 'a'? b[i] + 32:b[i];
        if (a[i] == b[i]) k = 2;
        else if (a[i] > b[i]) {k = 1;break;}
        else {k = 0;break;}
    }

    if (k == 2)
    {
        if (a.size() > b.size()) k = 1;
        else if (a.size() < b.size()) k = 0;
    }
    cout << op[k];
    return 0;
} 
// 题解2
#include <iostream>
#include <string>
#include <cstring>

using namespace std;

int main()
{
    string a,b;
    getline(cin,a);
    getline(cin,b);

    for (int i = 0; i < a.size(); i ++)
        if (a[i] >= 'A' && a[i] <= 'Z') a[i] += 32;

    for (int i = 0; i < b.size(); i ++)
        if (b[i] >= 'A' && b[i] <= 'Z') b[i] += 32;

    int c;
    c = strcmp(a.c_str(),b.c_str());
    if(c < 0) cout << "<";
    if(c == 0) cout << "=";
    if(c > 0) cout << ">";

    return 0;
}
此题如果使用string类型，也想用strcmp比较大小，可以使用string的成员函数c_str()把string转换为const char *，然后比较。
作者：paprika
链接：https://www.acwing.com/solution/content/12573/
来源：AcWing
```

## 2.acwing.766.去掉多余的空格

```C++
// 没有想到好的思路
// y总简单题解
#include <iostream>
using namespace std;
int main()
{
    string s;
    while (cin >> s) cout << s << ' ';// cin遇到空格停止
    
    return 0;
}

// y总题解2、3，自己过滤多余的空格
#include <iostream>
using namespace std;
int main()
{
    string s;
    getline(cin,s);
    string r;
    for (int i = 0;i < s.size();i++)
    {
        if (s[i] != ' ') r += s[i];
        else
        {
            r += ' ';
            int j = i;// 这三行代码是常用的算法，第一类双指针算法
            while (j < s.size() && s[j] == ' ') j++;
            i = j - 1;
        }
    }
    cout << r;
    return 0;
}
// 将26,27,28,29改成 if(!i || s[i-1] != ' ') r += s[i];也可以
// 加上!i避免i=0时索引越界
// 局部性判断方法
```

## 3.string!!!

标准库类型string，非常强大，大部分场合可用string而不用char数组。

一般情况下，不超时的话用string，如果输入量比较大则使用字符串。

![image-20210116183048237](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232013122.png)

string里有c_str()函数用于string转char数组。

使用方法为

```C++
string str;
printf("%s",str.c_str());
```

string.c_str()方法可以将string转换成char类型！！！（string为变量名）

puts() 也是一样，`puts(s.c_str());`

**注意：不能使用scanf读取string类型变量！！！**

![image-20210117092853170](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232013171.png)

empty函数：测试string是否为空（即长度是否为0），如果为空，输出1，否则输出0.这里的1,0代表真假。

size函数：求string长度，不像strlen函数会进行循环，它的时间复杂度是O(1)，很快。

![image-20210117093630721](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232013616.png)

`s4 = s1 + “asdafd” + ‘s’;// 这种也是支持的`

![image-20210117094315743](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232013308.png)

**注意：必须确保每个“+”的两侧至少有一个是string！！！**

上面s6正确是因为左边s1 + “,”已经变成了一个string，所以可以通过编译。

下面展示遍历string：

![image-20210117094823940](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232013028.png)

注意：上面这种遍历相当于是把s中的每个字符复制给c，并不会修改原s中的字符！！但是下面加上了&就可以进行修改，此时c完全等价于s中的字符，而且下面的效率更高一点！

## 4.acwing770.单词替换

 输入格式

输入共3行。

第1行是包含多个单词的字符串 s;

第2行是待替换的单词a(长度不超过100);

第3行是a将被替换的单词b(长度不超过100)。

共一行，输出将s中所有单词a替换成b之后的字符串。

输入样例：

```
You want someone to help you
You
I
```

 输出样例：

```
I want someone to help you
```

```C++
// solution 1，y总解法
#include <iostream>
#include <sstream>

using namespace std;
int main()
{
    string s,a,b;
    getline(cin,s);
    cin >> a >> b;
    stringstream ssin(s);// ssin从string流中读取string，类似cin
    string str;
    while （(ssin >> str){
     	if(str == a) {cout << b << ' ';
    	else cout << str << ' ';}   
    }
    return 0;
}
// solution 2
#include<bits/stdc++.h>
using namespace std;
string a[10000],b,d;
int main()
{
    int n=0,c=0;
    while(cin>>a[c])
    {
        c++;
        char x=getchar();
        if(x=='\n')
            break;
    }
    cin>>b>>d;
    for(int i=0;i<c;i++)
    {
        if(a[i]==b)
            cout<<d<<" ";
        else 
            cout<<a[i]<<" ";
    }
}

作者：SLL
链接：https://www.acwing.com/solution/content/12105/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### sstream讲解：

ssin用于从string字符串中提取出需要的信息，同样地有sscanf()函数。

大部分情况下还是用ssin，很少用到sscanf。

```C++
#include <iostream>
#include <sstream>
using namespace std;
int main()
{
    string s;
    getline(cin,s);
    stringstream ssin(s);// 把strnig初始化成string流
    // 用法类似cin
    int a,b;string str;double c;
    ssin >> a >> str >> b >> c;
    cout << a << endl << str << endl << b << endl <<c;
    return 0;
}
# 12 llx 23 23.34(输入)
# (输出)
12
llx
23
23.34
// char[]字符串的类似用法    
#include <cstdio>
#include <iostream>
#include <sstream>
using namespace std;
int main()
{
    char s[1000];
    fgets(s,1000,stdin);
    int a,b;
    char str[1000];
    double c;
    sscanf(s,"%d%s%d%lf",&a,str,&b,&c);
    printf("%d\n%s\n%d\n%lf\n",a,str,b,c);
    return 0;
}
```

## 5.acwing.771.字符串中最长的连续出现的字符

又不会了，字符串很多题都没有思路。

这题用到第一类双指针算法，前面已经提过！

求一个字符串中最长的连续出现的字符，输出该字符及其出现次数，字符串中无空白字符（空格、回车和tab），如果这样的字符不止一个，则输出第一个。

输入格式

第一行输入整数N，表示测试数据的组数。

每组数据占一行，包含一个不含空白字符的字符串，字符串长度不超过200。

输出格式

共一行，输出最长的连续出现的字符及其出现次数，中间用空格隔开。

 输入样例：

```
2
aaaaabbbbbcccccccdddddddddd
abcdefghigk
```

输出样例：

```
d 10
a 1
```

```C++
// y总题解
#include <iostream>

using namespace std;
int main()
{
    int num;
    cin >> num;
    while (num--)
    {
        string s;
        cin >> s;
        int cnt = 0;char c;
        for (int i = 0;i < s.size();i++)
        {
            int j = i;// 第一类双指针算法
            while (j <s.size() && s[i] == s[j]) j++;
            // 此时j是和s[i]不相等的第一个字符的下标
            if (j - i > cnt) {cnt = j-i;c = s[i];}
            i = j - 1;// 特别注意是i-1，因为循环中有i++
        }
        cout << c << ' ' << cnt << endl;
    }
    
    return 0;
}
```

## 6.acwing.774.最长单词

一个以’.’结尾的简单英文句子，单词之间用空格分隔，没有缩写形式和其它特殊形式，求句子中的最长单词。

输入格式

输入这个简单英文句子，长度不超过500。

 输出格式

该句子中最长的单词。如果多于一个，则输出第一个。

 输入样例：

```
I am a student of Peking University.
University
```

输出样例：

```C++
// solution 1，自己的解法
#include <iostream>
#include <string>
using namespace std;
string s[500];
int main()
{
    int i = 0,p = 0,len = 0;
    while (cin >> s[i]) i++;
    for (int j = 0;j < i;j++)
    {
        int t = s[j].size();
        if (j == i - 1) t -= 1; 
        if (len < t) {len = t;p = j;}
    }
    if (p == i - 1)
    {
        for (int k = 0;k < s[p].size() - 1;k++)
        {
            cout << s[p][k];
        }
    }
    else cout << s[p];
    return 0;
}
// solution 2，y总解法
#include <iostream>
using namespace std;

int main()
{
    string res,str;
    while (cin >> str)
    {
        if (str.back() == '.') str.pop_back();
        // str.back()函数返回string的最后一个字符
        // str.pop_back()去掉string的最后一个字符
        if (str.size() > res.size()) res = str;
    }
    cout << res << endl;
    return 0;
}
// solution 3，双指针算法，字符串中的常用算法
#include <iostream>
#include <algorithm>
#include <string.h>

using namespace std;

int main()
{
    string a;
    getline(cin, a);
    int max = 0, tag_i, tag_j;
    for(int i = 0; i < a.size(); i ++)
    {
        int j = i;
        while(j < a.size() && a[j] != ' ') j ++;
        if(max < j - i)
        {
            max = j - i + 1;
            tag_i = i;
            tag_j = j;
        }
        i = j;
    }

    for(int i = tag_i; i <= tag_j && a[i] != '.'; i ++) cout << a[i];
	// 这里的max并不是单词最大长度，结果多输出了一个空格
    return 0;

}

作者：Logic
链接：https://www.acwing.com/solution/content/11742/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## 7.acwing.775.倒排单词

编写程序，读入一行英文(只包含字母和空格，单词间以单个空格分隔)，将所有单词的顺序倒排并输出，依然以单个空格分隔。

输入格式

输入为一个字符串（字符串长度至多为100）。

输出格式输出为按要求排序后的字符串。

输入样例：

```
I am a student
```

输出样例：

```
student a am I
```

```C++
// solution 1
#include <iostream>
using namespace std;

int main()
{
    string str, res;
    while (cin >> str)
        res = str + ' ' + res;
    cout << res;

}
// solution 2，y总题解
#include <iostream>
using namespace std;
int main()
{
    string str[100];
    int n = 0;
    while (cin >> str[n]) n ++;
    for (int i = n-1;i >= 0;i --) cout << str[i] << ' ';
    cout << endl;
    return 0;
}
```

## 8.acwing.776.字符串移位包含问题

对于一个字符串来说，定义一次循环移位操作为：将字符串的第一个字符移动到末尾形成新的字符串。

给定两个字符串s1和s2，要求判定其中一个字符串是否是另一字符串通过若干次循环移位后的新字符串的子串。

例如CDAA是由AABCD两次移位后产生的新串BCDAA的子串，而ABCD与ACBD则不能通过多次移位来得到其中一个字符串是新串的子串。

 输入格式

共一行，包含两个字符串，中间由单个空格隔开。

字符串只包含字母和数字，长度不超过30。

 输出格式

如果一个字符串是另一字符串通过若干次循环移位产生的新串的子串，则输出true，否则输出false。

输入样例：

```
AABCD CDAA
```

输出样例：

```
true
```

难度：困难，不知道怎么判断子串。（y总：本章压轴题，最难的问题之一）

```c++
#include <iostream>
#include <algorithm>
using namespace std;
int main()
{
    string a,b;cin >> a >> b;
    if (a.size() < b.size()) swap(a,b);
    for (int i = 0;i < a.size();i++)
    {
        a = a.substr(1) + a[0];
        for (int j = 0;j + b.size() <= a.size();j++)
        {
            int k;
            for (k = 0;k < b.size();k++)
            {
                if(a[j+k] != b[k])
                {
                    break;
                }
            }
            if (k == b.size())
            {
                puts("true");
                return 0;
            }
        }
    }
    puts("false");
    return 0;
}
```

## 9.acwing.777.字符串乘方

又不会了！困难题。

给定两个字符串a和b,我们定义a*b为他们的连接。

例如，如果a=”abc” 而b=”def”， 则a*b=”abcdef”。 

如果我们将连接考虑成乘法，一个非负整数的乘方将用一种通常的方式定义：

a$^0$=””(空字符串)，a$^{(n+1)}$=a∗(a$^n$)。

输入格式

输入包含多组测试样例，每组测试样例占一行。

每组样例包含一个字符串s，s的长度不超过100。

最后的测试样例后面将是一个点号作为一行。

输出格式

对于每一个s，你需要输出最大的n，使得存在一个字符串a，让s=ans=an。

输入样例：

```
abcd
aaaa
ababab
.
```

输出样例：

```
1
4
3
```

```c++
// y总题解
#include <iostream>
#include <string>

using namespace std;
int main()
{
    string s;
    while(cin >> s,s != ".")
    {
        int len = s.size();
        for (int n = len; n;n--)// n从大到小枚举，因为求最大的n值
        {
            if (len % n == 0)
            {
                int m = len / n;
                string str = s.substr(0,m);
                string res;
                for (int i = 0; i < n; i++) res += str;
                if (res == s)
                {
                    cout << n << endl;
                    break;
                }
            }
        }
    }
    return 0;
}
```

## 10.acwing.778.字符串最大跨距

难度确实不小，又白给了:slightly_smiling_face:大概思路还是对的，具体代码细节不会了:cold_sweat:

有三个字符串S,S1,S2，其中，S长度不超过300，S1和S2的长度不超过10。

现在，我们想要检测S1和S2是否同时在S中出现，且S1位于S2的左边，并在S中互不交叉（即，S1的右边界点在S2的左边界点的左侧）。

计算满足上述条件的最大跨距（即，最大间隔距离：最右边的S2的起始点与最左边的S1的终止点之间的字符数目）。

如果没有满足条件的S1，S2存在，则输出-1。

例如，S = “abcd123ab888efghij45ef67kl”, S1=”ab”, S2=”ef”，其中，S1在S中出现了2次，S2也在S中出现了2次，最大跨距为：18。

输入格式

输入共一行，包含三个字符串S,S1,S2，字符串之间用逗号隔开。

数据保证三个字符串中不含空格和逗号。

输出格式

输出一个整数，表示最大跨距。

如果没有满足条件的S1和S2存在，则输出-1.

输入样例：

```
abcd123ab888efghij45ef67kl,ab,ef
```

输出样例：

```
18
```

```C++
// y总题解
#include <iostream>
#include <string>
#include <cstdio>

using namespace std;
int main()
{
    string s,s1,s2;
    char c;
    while (cin >> c,c != ',') s += c;
    while (cin >> c,c != ',') s1 += c;
    while (cin >> c) s2 += c;

    if (s.size() <= s1.size() || s.size() <= s2.size()) puts("-1");
    else
    {
        int l = 0;
        while (l + s1.size() <= s.size())// 这里用到的结构与上一题类似
        {
            int k = 0;
            while (k < s1.size())
            {
                if (s1[k] != s[l+k]) break;
                k ++;
            }
            if (k == s1.size()) break;
            l ++;
        }

        int r = s.size() - s2.size();
        while (r >= 0)
        {
            int k = 0;
            while (k < s2.size())
            {
                if (s2[k] != s[r+k]) break;
                k ++;
            }
            if (k == s2.size()) break;
            r --;
        }

        l += s1.size() -1;
        if (l >= r) puts("-1");
        else printf("%d",r - l -1);
    }
    
    return 0;
}
```

***str.find()简介\***

查找第一次出现的目标字符串：

```C++
#include<iostream>
#include<cstdio>
using namespace std;
int main(){
    string s1 = "abcdef";
    string s2 = "de";
    int ans = s1.find(s2) ;   //在S1中查找子串S2
    cout<<ans<<endl;
    system("pause");
}
```

说明：如果查找成功则输出查找到的第一个位置，否则返回-1. 

***查找从指定位置开始的第一次出现的目标字符串：***

```C++
#include<iostream>
#include<csdtio>
using namespace std;
int main(){
    string s1 = "abcdef";
    string s2 = "de";
    int ans = s1.find(s2, 2) ;   //从S1的第二个字符开始查找子串S2
    cout<<ans<<endl;
    system("pause");
}
```

```C++
// solution 2，使用了STL中的find函数
#include<iostream>
#include<string>
using namespace std;
#include<stdio.h>
#include<string.h>
int main()
{

    string S,S1,S2;
    int i=0;
    char a;
    while(cin>>a)
    {
        if(a!=',')
        {
            if(i==0) S+=a;
            else if(i==1) S1+=a;
            else S2+=a;
        }
        else i++;
    }
    int s,s1,s2;
    s1=S.find(S1);
    s2=S.rfind(S2);
    if(s1!=-1&&s2!=-1&&(s1+S1.length()-1)<s2){
        cout<<s2-s1-S1.length()<<endl;
    }
    else cout<<"-1"<<endl;

}

作者：Sir.Guo
链接：https://www.acwing.com/solution/content/7213/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```