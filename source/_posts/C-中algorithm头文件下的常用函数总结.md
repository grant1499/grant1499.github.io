---
title: C++中algorithm头文件下的常用函数总结
tags:
  - STL
categories:
  - - C++
mathjax: true
copyright: true
abbrlink: d38917ef
date: 2021-04-23 13:27:47
---

这块内容在STL的文章中都已经介绍过了，但还是单独提下algorithm的内容。

参考自：https://blog.csdn.net/qq_39445165/article/details/94721121

详细内容可以参照《算法笔记》。

<!--more-->

## ① max、min和abs函数，下面是具体的代码：

```C++
#include<stdio.h>
#include<algorithm>
using namespace std;
int main(void){
	int x = 1, y = -2;
	printf("%d %d\n", max(x, y), min(x, y));
	printf("%d %d\n", abs(x), abs(y));
	return 0;
}
```

## ② swap函数，下面是具体的代码：

```C++
#include<stdio.h>
#include<algorithm>
using namespace std;
int main(void){
	int x = 1, y = -2;
	swap(x, y);
	printf("%d %d\n", x, y);
	return 0;
}
```

## ③ reverse函数

reverse（it，it2）可以将数组指针在[it，it2）之间的元素或者容器的迭代器在[it，it2）范围内的元素进行翻转，具体的代码如下：

```C++
#include<stdio.h>
#include<algorithm>
using namespace std;
int main(void){
	int arr[] = {1, 2, 3, 4, 5, 6, 7, 7};
	reverse(arr, arr + 8);
	for(int i = 0; i < 8; ++i){
		printf("%d ", arr[i]);
	}
	return 0;
}
```

## ④ next_permutation函数

使用这个函数可以生成给定序列在全排列中的下一个排列，并且生成的排列是从小到大进行排序的，具体的代码如下：

```C++
#include<stdio.h>
#include<algorithm>
using namespace std;
int main(void){
	int arr[] = {1, 2, 3, 4};
	//使用C++中的net_permutation函数生成数组的全排列
	do{
		printf("%d%d%d%d\n", arr[0], arr[1], arr[2], arr[3]);
	}while(next_permutation(arr, arr + 4));
	return 0;
}
```

## ⑤ fill函数

fill函数可以将数组或者是容器中的某一区间赋予某个相同的值，与memset函数不同的是，这里的赋值可以是数组类型对应范围中的任意值，具体的代码如下：

```C++
#include<stdio.h>
#include<algorithm>
using namespace std;
int main(void){
	int arr[] = {1, 2, 3, 4, 5, 6, 7, 7};
	fill(arr, arr + 8, 100);
	for(int i = 0; i < 8; ++i){
		printf("%d ", arr[i]);
	}
	return 0;
}
```

