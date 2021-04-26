---
title: two pointers思想与归并排序
date: 2021-04-17 19:49:40
tags: [排序,tow pointers]
categories: 
	- [C++]
	- [算法]
mathjax: true
copyright: true
---

参考自：https://www.cnblogs.com/coderJiebao/p/Algorithmofnotes08.html

## 什么是 two pointers

以一个例子引入：给定一个递增的正整数序列和一个正整数 M，求序列中的两个不同位置的数 a 和 b，使得它们的和恰好为 M，输出所有满足条件的方案。

　　本题的一个最直观的想法是，使用二重循环枚举序列中的整数 a 和 b，判断它们的和是否为 M。时间复杂度为 O(n^2)。当n的规模足够大时，这显然是不可取的。

<!--more-->

two pointers 将利用有序序列的枚举特性来有效降低复杂度。它针对本题的算法如下：

令下标 i 的初值为0，下标 j 的初值为 n-1，即令 i、j 分别指向序列的第一个元素和最后一个元素，接下来根据 a[i]+a[j] 与 M 的大小来进行下面三种选择，使 i 不断向右移动、使 j 不断向左移动，直到 i≥j 成立

- 若 a[i]+a[j]==M ，说明找到了其中一种方案，令 i=i+1、j=j-1。

- 若 a[i]+a[j]>M，令 j=j-1。

- 若 a[i]+a[j]<M，令 i=i+1。

　　反复执行上面三个判断，直到 i≥j 成立，在递增序列的前提下，循环只需要进行到i>=j时停止，所以理想状态下只需要遍历半个序列，**时间复杂度只需要O(n)**。

代码如下：

```C++
while(i < j) {
    if(a[i]+a[j] == M) {
        printf("%d %d\n", i, j);
        i++; j--;
    } else if(a[i]+a[j] < M) {
        i++;
    } else {
        j--;
    }
}
```

## 序列合并问题

再来看**序列合并问题**。假设有两个递增序列 A 与 B，要求将它们合并为一个递增序列 C。

　　同样的，可以设置两个下标 i 和 j ，初值均为0，表示分别指向序列 A 的第一个元素和序列 B 的第一个元素，然后根据 A[i] 与 B[j] 的大小来决定哪一个放入序列 C。

- 若 A[i]≤B[j]，把 A[i] 加入序列 C 中，并让 i 加1
- 若 A[i]>B[j]，把 B[j] 加入序列 C 中，并让 j 加1

上面的分支操作直到 i、j 中的一个到达序列末端为止，然后将另一个序列的所有元素依次加入序列 C 中，代码如下：

```C++
int merge(int A[], int B[], int C[], int n, int m) {
    int i=0, j=0, index=0;    // i指向A，j指向B，index指向C
    while(i<n && j<m) {
        if(A[i] <= B[j]) {            // 若 A[i]≤B[j]
            C[index++] = A[i++];
        } else {                    // 若 A[i]>B[j]
            C[index++] = B[j++];
        }
    }
    while(i<n)    C[index++] = A[i++];    // 若 A 有剩余
    while(j<m)    C[index++] = B[j++];    // 若 B 有剩余
    return index;        // 返回 C 长度
}
```

广义上的 two pointers 是利用问题本身与序列的特性，使用两个下标 i、j 对序列进行扫描（可以同向扫描，也可以反向扫描），以较低的复杂度（一般为 O(n) ）解决问题。 

## 归并排序

归并排序是一种基于“归并”思想的排序方法，本节主要介绍其中最基本的 2-路归并排序。2-路归并排序的原理是，将序列两两分组，将序列归并为$\lceil \frac n 2 \rceil$个组，组内单独排序；然后将这些组再两两归并，生成$\lceil \frac n 4 \rceil$个组，组内再单独排序；以此类推，直到只剩下一个组为止。时间复杂度为 O(nlogn)。

说明：对于偶数个的数组正常对半分就行，对于奇数个的数组，留下最后1个多余的单独1组，其余两两1组。

### 1. 递归实现

只需反复将当前区间 [left,right] 分为两半，对两个子区间 [left,mid] 与 [mid+1, right] 分别递归进行归并排序，然后将两个已经有序的子区间合并为有序序列即可。代码如下：

```C++
const int maxn = 100;
// 将数组A的 [L1,R1] 与 [L2,R2] 合并为有序区间（此处 L2=R1+1 ）
void merge(int A[], int L1, int R1, int L2, int R2) {
    int i=L1, j=L2;
    int temp[maxn], index=0;    // temp 临时储存合并序列
    while(i<=R1 && j<=R2) {
        if(A[i] <= A[j]) {            // 若 A[i] ≤A[j]
            temp[index++] = A[i++];
        } else {                    // 若 A[i] > A[j]
            temp[index++] = A[j++];
        }
        while(i <= R1) temp[index++] = A[i++];
        while(j <= R2) temp[index++] = A[j++];
        for(int i=0; i<index; ++i) {
            A[L1+i] = temp[i];    // 将合并后的序列赋值回 A
        }
    }
}
// 归并排序递归实现
// 只需反复将当前区间 [left,right] 分为两半，对两个子区间 [left,mid] 与 [mid+1, right]
// 分别递归进行归并排序，然后将两个已经有序的子区间合并为有序序列即可。
void mergeSort(int A[], int left, int right) {
    if(left < right) {    // 当 left==right 时，只有一个元素，认定为有序
        int mid = (left+right)/2;
        mergeSort(A, left, mid);            // 分为左区间和右区间
        mergeSort(A, mid+1, right);
        merge(A, left, mid, mid+1, right);    // 将左区间和右区间合并
    }
}
```

### 2.非递归实现

非递归实现主要考虑到这样一点：每次分组时组内元素个数上限都是2的幂次。于是就可以想到这样的思路：令步长 step 的初值为2，然后将数组中每 step 个元素作为一组，将其内部进行排序；再令 step 乘以2，重复上面的操作，直到 step/2 超过元素个数 n 。代码如下：

```C++
const int maxn = 100;

// 将数组A的 [L1,R1] 与 [L2,R2] 合并为有序区间（此处 L2=R1+1 ）
void merge(int A[], int L1, int R1, int L2, int R2) {
    int i=L1, j=L2;
    int temp[maxn], index=0;    // temp 临时储存合并序列
    while(i<=R1 && j<=R2) {
        if(A[i] <= A[j]) {            // 若 A[i] ≤A[j]
            temp[index++] = A[i++];
        } else {                    // 若 A[i] > A[j]
            temp[index++] = A[j++];
        }
        while(i <= R1) temp[index++] = A[i++];
        while(j <= R2) temp[index++] = A[j++];
        for(int i=0; i<index; ++i) {
            A[L1+i] = temp[i];    // 将合并后的序列赋值回 A
        }
    }
}

// 归并排序非递归实现
// 令步长 step 的初值为2，然后将数组中每 step 个元素作为一组，
// 将其内部进行排序；再令 step 乘以2，重复上面的操作，直到 step/2 超过元素个数 n 。
void mergeSort(int A[]) {
    // step 为组内元素个数
    for(int step=0; step/2 <= n; step *= 2) {
        for(int i = 1; i <= n; i += step) {    // 对每一组，数组下标从1开始
            int mid = i + step/2 -1;    // 左区间元素个数为 step/2
            if(mid+1 <= n) {    // 右区间存在元素
                // 左区间为 [left,mid]，右区间为 [mid+1, min(i+step-1,n)
                merge(A, i, mid, mid+1, min(i+step-1, n));
            }

            /*
            // 28-32行也可以用 sort 代替 merge 函数，只要时间允许
            sort(A+i, A+min(i+step, n+1));
            */
        }
        // 用 sort 代替，此处输出归并排序的某一趟结束时的序列
    }
}
```

如果题目只要求给出归并排序每一趟结束时的序列，可以用sort函数代替 merge 函数，只要时间允许。

