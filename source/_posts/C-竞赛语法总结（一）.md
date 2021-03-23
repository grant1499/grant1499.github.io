---
title: C++竞赛语法总结（一）
date: 2021-02-24 07:01:57
tags: [竞赛语法]
categories: 
	- [C++]
	- [算法]
mathjax: true
copyright: true
---

## 1.acwing656.钞票和硬币

读取一个带有两个小数位的浮点数，这代表货币价值。

在此之后，将该值分解为多种钞票与硬币的和，每种面值的钞票和硬币使用数量不限，要求使用的钞票和硬币的数量尽可能少。

<!--more-->

钞票的面值是100,50,20,10,5,2。

硬币的面值是1,0.50,0.25,0.10,0.05和0.01。

输入格式

输入一个浮点数N。

输出格式

参照输出样例，输出每种面值的钞票和硬币的需求数量。

数据范围

0≤N≤1000000.00

输入样例：

```
576.73
```

输出样例：

```c++
NOTAS:
5 nota(s) de R$ 100.00
1 nota(s) de R$ 50.00
1 nota(s) de R$ 20.00
0 nota(s) de R$ 10.00
1 nota(s) de R$ 5.00
0 nota(s) de R$ 2.00
MOEDAS:
1 moeda(s) de R$ 1.00
1 moeda(s) de R$ 0.50
0 moeda(s) de R$ 0.25
2 moeda(s) de R$ 0.10
0 moeda(s) de R$ 0.05
3 moeda(s) de R$ 0.01
```

```c++
//读取一个浮点数，表示货币价值，将他分解为多种钞票和硬币的和，尽可能少的钞票和硬币数量
#include <cstdio>

using namespace std;

int main()
{
    int a,c;
    double b;
    scanf("%lf",&b);
    a = (int)b;
    c = 100*b - 100*a;// 注意这里
    printf("NOTAS:\n");
    printf("%d nota(s) de R$ 100.00\n",a/100);
    a %= 100;
    printf("%d nota(s) de R$ 50.00\n",a/50);
    a %= 50;
    printf("%d nota(s) de R$ 20.00\n",a/20);
    a %= 20;
    printf("%d nota(s) de R$ 10.00\n",a/10);
    a %= 10;
    printf("%d nota(s) de R$ 5.00\n",a/5);
    a %= 5;
    printf("%d nota(s) de R$ 2.00\n",a/2);
    a %= 2;

    printf("MOEDAS:\n");
    printf("%d moeda(s) de R$ 1.00\n",a/1);

    printf("%d moeda(s) de R$ 0.50\n",c/50);
    c %= 50;
    printf("%d moeda(s) de R$ 0.25\n",c/25);
    c %= 25;
    printf("%d moeda(s) de R$ 0.10\n",c/10);
    c %= 10;
    printf("%d moeda(s) de R$ 0.05\n",c/5);
    c %= 5;
    printf("%d moeda(s) de R$ 0.01",c/1);
    return 0;
}
```

## 2.acwing610.工资和奖金

请你编写一个程序，给定你一个销售人员的名字，底薪以及月销售额。

请你计算他的月收入是多少。

已知月收入等于底薪加15%的月销售额。

所有数据保留两位小数。

输入格式

输入第一行包含一个由大写字母构成的长度不超过10的字符串，表示销售人员的名字。

第二行包含一个浮点数，表示该人员的底薪。

第三行包含一个浮点数，表示该人员的月销售额。

输出格式

输出格式为“TOTAL = R$ X”，X为该人员月收入。

数据范围

0≤底薪，月销售额≤10000.00

输入样例：

```
JOAO
500.00
1230.30
```

输出样例：

```
TOTAL = R$ 684.54
```

```c++
//重点在读取字符串的区别
// solution 1
#include <cstdio>

using namespace std;

int main()
{
    double a,b;
    char s[10];
    scanf("%s %lf %lf",s,&a,&b);
    printf("TOTAL = R$ %.2lf",a+0.15*b);
    return 0;
}

//solution 2
#include <cstdio>
#include <iostream>
using namespace std;

int main()
{
    string name;
    cin >> name;
    double x,y;
    cin >> x >> y;

    printf("TOTAL = R$ %.2lf",x+0.15*y);
    return 0;
}
```

## 3.解题新思路

```c++
//交换a和b,c的值使a,b,c按从大到小排列
// solution 1
if (a < b)
    {
        t = a;a = b;b = t;
    }
    if (b < c)
    {
        t = b;b = c;c = t;
        if (a < b)
        {
            t = a;a = b;b = t;
        }
    }
    
// solution 2
<algorithm>
if (a < b) swap(a,b);
if (a < c) swap(a,c);
if (b < c) swap(b,c);
```

## 4.acwing668.游戏时间2

读取四个整数A,B,C,D，用来表示游戏的开始时间和结束时间。

其中A和B为开始时刻的小时和分钟数，C和D为结束时刻的小时和分钟数。

请你计算游戏的持续时间。

比赛最短持续1分钟，最长持续24小时。

输入格式

共一行，包含四个整数A,B,C,D。

输出格式

输出格式为“O JOGO DUROU X HORA(S) E Y MINUTO(S)”，表示游戏共持续了X小时Y分钟。

数据范围

0≤A,C≤23
		0≤B,D≤59

输入样例1：

```
7 8 9 10
```

输出样例1：

```
O JOGO DUROU 2 HORA(S) E 2 MINUTO(S)
```

输入样例2：

```
7 7 7 7
```

输出样例2：

```
O JOGO DUROU 24 HORA(S) E 0 MINUTO(S)
```

输入样例3：

```
7 10 8 9
```

输出样例3：

```
O JOGO DUROU 0 HORA(S) E 59 MINUTO(S)
```

```c++
//读取游戏的开始和结束小时和分钟数，估算持续时间
//注意一些样例的特殊性
#include<stdio.h>
int main(void)
{
    int a,b,c,d;
    scanf("%d%d%d%d",&a,&b,&c,&d);
    b+=a*60,d+=c*60;
    a=(d-b+24*60)%(24*60); 
    a=(a)?(a):(24*60);// 当a=0时，a变成24*60，处理特殊样例
    printf("O JOGO DUROU %d HORA(S) E %d MINUTO(S)\n",a/60,a%60);
    return 0;
}
```

## 5.acwing663.简单排序

```c++
//输入三个数对他们升序排列每行输出一个数，接着一个空行，按原顺序每行输出一个数
// solution 1
#include<cstdio>
#include<iostream>
using namespace std;
int main()
{
    int a,b,c;
    cin>>a>>b>>c;
    int x,y,z;
    x=max(a,max(b,c));
    y=min(a,min(b,c));
    z=a+b+c-x-y;
    cout<<y<<endl;
    cout<<z<<endl;
    cout<<x<<endl;
    cout<<endl;
    cout<<a<<endl;
    cout<<b<<endl;
    cout<<c<<endl;
    return 0;
}

// solution 2
#include <cstdio>

using namespace std;

int main()
{
    int a,b,c,t,d,e,f;
    scanf("%d %d %d",&a,&b,&c);
    d = a;e = b;f = c;
    if (a<b)
    {
        t = a;a = b;b = t;
    }
    if (b<c)
    {
        t = b;b = c;c = t;
        if (a<b)
        {
            t = a;a = b;b = t;
        }
    }

    printf("%d\n%d\n%d\n\n",c,b,a);
    printf("%d\n%d\n%d\n",d,e,f);
    return 0;
}
// solution 2
<algorithm>
if (a < b) swap(a,b);
if (a < c) swap(a,c);
if (b < c) swap(b,c);
```

## 6.acwing714.连续奇数和1

```c++
//给定两整数x和y，输出他们之间（不包括x，y）的所有奇数和
#include <cstdio>
#include <algorithm>
using namespace std;

int main()
{
    int x,y,sum = 0;
    scanf("%d %d",&x,&y);
    if (x > y) swap(x,y);
    for (int i = x + 1;i < y;i++)
    {
        if (i%2) sum += i;
    }
    printf("%d",sum);
    return 0;
}
// 注意：-1%2 = -1,-3%2 = -1,-2%2 = 0
```

## 7.acwing721.递增序列

```c++
//读取一系列的整数X，对于每个X，输出一个1,2,…,X的序列
//其中最后一个为0，其他的均为正整数,对于最后一行的整数0，不作任何处理
//solution 1
#include <cstdio>
#include <iostream>
using namespace std;

int main()
{
    int x;
    while (cin >> x,x)
    {
        for (int i = 1;i <= x;i++)
        {
            printf("%d ",i);
        }
        cout << endl;
    }
    return 0;
}

//solution 2
#include<stdio.h>
int main(int n){
    while(~scanf("%d",&n),n){
        for(int i=1;i<=n;i++) printf("%d ",i);
        puts("");// 输出字符串，遇'\0'停止，自动加换行
    }
    return 0;
}
//~scanf("%d",&n)等价于scanf("%d",&n) != -1(EOF)
```

## 8.acwing710.六个奇数

```c++
// 读取一个整数X，输出X之后的6个奇数，如果X也是奇数，那么它也算作6个奇数之一
//每个数占一行
#include <iostream>

using namespace std;

int main()
{
    int x;
    cin >> x;

    for (int i = x, j = 0; j < 6; i ++ )
        if (i % 2)
        {
            cout << i << endl;
            j ++ ;
        }

    return 0;
}
```

## 9.acwing719.连续奇数和2

```c++
//输入N对整数对X,Y，对于每对X,Y，请你求出它们之间（不包括X和Y）的所有奇数的和
//solution 1
#include <cstdio>
#include <algorithm>
using namespace std;

int main()
{
    int n;
    scanf("%d",&n);
    for (int i = 1;i <= n;i++)
    {
        int x,y,sum = 0;
        scanf("%d %d",&x,&y);
        if (x > y) swap(x,y);
        for (int i = x + 1;i < y;i++)
        {
            if (i%2) sum += i;
        }
        printf("%d\n",sum);
    }
    return 0;
}

//solution 2
#include<stdio.h>
int x,y,ans,t;
int main(){
    scanf("%d",&t);
    while(t--){
        ans=0;
        scanf("%d%d",&x,&y);
        for(int i=(x<y?x:y)+1;i<(x>y?x:y);i++) if(i&1) ans+=i;
        //i&1 用于判断奇偶，为真是奇数
        printf("%d\n",ans);
    }
}
```

## 10.acwing717.简单斐波那契

```c++
//以下数列0 1 1 2 3 5 8 13 21 …被称为斐波纳契数列
//在一行中输出斐波那契数列的前N项，数字之间用空格隔开
//solution 1
#include <cstdio>
#include <algorithm>
using namespace std;

int main()
{
    int n;
    scanf("%d",&n);
    int a = 0,b = 1,c = a + b;
    for (int i = 0;i < n;i++)
    {
        if (i == 0) printf("0 ");
        else if (i == 1) printf("1 ");
        else
        {
            printf("%d ",c);
            a = b;
            b = c;
            c = a+b;
        }

    }
    return 0;
}
//better edition
//solution 2 
#include<stdio.h>
long long n,a,b=1,c;// 这里a自动初始化为0
int main(){
    scanf("%d",&n);
    while(n--){
        printf("%d ",a);
        c=a+b;
        a=b,b=c;
    }
    return 0;
}
```