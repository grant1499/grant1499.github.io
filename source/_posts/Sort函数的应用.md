---
title: Sort函数的应用
tags:
  - 排序
categories:
  - - C++
mathjax: true
copyright: true
abbrlink: e6c1070a
date: 2021-03-23 22:00:11
---

## PAT 甲级 1025

<!--more-->

Programming Ability Test (PAT) is organized by the College of Computer Science and Technology of Zhejiang University. Each test is supposed to run simultaneously in several places, and the ranklists will be merged immediately after the test. Now it is your job to write a program to correctly merge all the ranklists and generate the final rank.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive number *N* (≤100), the number of test locations. Then *N* ranklists follow, each starts with a line containing a positive integer *K* (≤300), the number of testees, and then *K* lines containing the registration number (a 13-digit number) and the total score of each testee. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, first print in one line the total number of testees. Then print the final ranklist in the following format:

```
registration_number final_rank location_number local_rank
```

The locations are numbered from 1 to *N*. The output must be sorted in nondecreasing order of the final ranks. The testees with the same score must have the same rank, and the output must be sorted in nondecreasing order of their registration numbers.

### Sample Input:

```in
2
5
1234567890001 95
1234567890005 100
1234567890003 95
1234567890002 77
1234567890004 85
4
1234567890013 65
1234567890011 25
1234567890014 100
1234567890012 85
```

### Sample Output:

```out
9
1234567890005 1 1 1
1234567890014 1 2 1
1234567890001 3 1 2
1234567890003 3 1 2
1234567890004 5 1 4
1234567890012 5 2 2
1234567890002 7 1 5
1234567890013 8 2 3
1234567890011 9 2 4
```

### 题目大意

  给所有地区的考生排名。题目首先给出N，表示有N个地区，之后每个地区给出K，及K个考生的考号，分数，最后要求你不仅要输出每个考生的总排名，还要输出每个考生的地区排名

### 思路

  首先建立一个结构体，存储考生的考号，分数，地区序号及地区排名，然后建立一个充分大(>=30000)的结构体数组，然后依次每输入一个地区的考生信息，就对该地区的考试进行排序，同时计算其地区排名。输入完毕后，对该结构体数组进行总排序，最终计算总排名并输出

```c++
#include <cstring>
#include <algorithm>
#include <cstdio>
#include <iostream>
using namespace std;
const int N = 30010;
struct Student{
    int score;
    char id[14];
    int location_num;
    int local_rank;
}stu[N];
bool cmp(Student a,Student b)
{
    if (a.score != b.score) return a.score > b.score;
    else{
        return strcmp(a.id,b.id) < 0;
    }
}
int main()
{
    int num = 0;
    int n,m;
    scanf("%d",&n);
    for (int i = 0;i < n;i ++)
    {
        scanf("%d",&m);
        for (int j = 0;j < m;j ++)
        {
            scanf("%s %d",stu[num].id,&stu[num].score);
            stu[num].location_num = i+1;
            num++;
        }
        
        sort(stu+num-m,stu+num,cmp);
        stu[num-m].local_rank = 1;
        for (int i = num-m+1;i < num;i++)
        {
            if (stu[i].score == stu[i-1].score) stu[i].local_rank = stu[i-1].local_rank;
            else{
                stu[i].local_rank = i+1-(num-m);
            }
        }
    }
    printf("%d\n",num);
    sort(stu,stu+num,cmp);
    int r = 1;
    for (int i = 0;i < num;i ++)
    {
        if (i > 0 && stu[i].score != stu[i-1].score) r = i +1;
        
        printf("%s %d %d %d\n",stu[i].id,r,stu[i].location_num,stu[i].local_rank);
    }
    
    return 0;
}
```
