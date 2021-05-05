---
title: C++的输入输出速度比较
categories:
  - - C++
mathjax: true
copyright: true
abbrlink: 48fab36
date: 2021-03-11 22:54:35
---

下面对C++中的一些常用输入输出方式速度进行比较。

<!--more-->

众所周知，使用printf/scanf的速度会比cin/cout快上不少。

关于输出换行（回车）的几种方式：

1.cout << endl;

2.puts("");

3.printf("\n");

4.putchar(10);

```c++
#include <cstdio>
#include <iostream>
using namespace std;
int main()
{
    int n;
    cin >> n;
    for (int i = 0;i < n;i ++) cout << endl;
    return 0;
}
// 1290ms
```

```c++
#include <cstdio>
#include <iostream>
using namespace std;
int main()
{
    int n;
    cin >> n;
    for (int i = 0;i < n;i ++) puts("");
    return 0;
}
// 19ms
```

```c++
#include <cstdio>
#include <iostream>
using namespace std;
int main()
{
    int n;
    cin >> n;
    for (int i = 0;i < n;i ++) printf("\n");
    return 0;
}
// 10ms
```

```c++
#include <cstdio>
#include <iostream>
using namespace std;
int main()
{
    int n;
    cin >> n;
    for (int i = 0;i < n;i ++) putchar(10);
    return 0;
}
// 10ms
```

结论：

`printf("\n");`和`putchar(10);`最快，两者差距不大；

`puts("");`比上面的稍慢；

`cout << endl;`最慢，用时较长。

据大佬总结的endl的好处：

- endl好处是能一直刷新流，因为要刷新适合一些交互题；
- 而且如果是大工程，endl更容易方便debug