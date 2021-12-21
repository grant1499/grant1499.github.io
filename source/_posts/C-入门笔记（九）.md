---
title: C++入门笔记（九）
tags:
  - C++入门
  - 抽象类
categories:
  - C++
mathjax: true
copyright: true
abbrlink: 81b972f6
date: 2021-04-15 13:04:30
---

## 1.纯虚函数与抽象类

### 1.1纯虚函数

**首先：强调一个概念**

定义一个函数为虚函数，不代表函数为不被实现的函数。

定义他为虚函数是为了允许用基类的指针来调用子类的这个函数。

定义一个函数为纯虚函数，才代表函数没有被实现。

定义纯虚函数是为了**实现一个接口**，起到一个规范的作用，规范继承这个类的程序员必须实现这个函数。

<!--more-->

![image-20210415131524794](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232018203.png)

### 1.2抽象类

类似Java中抽象方法与抽象类的区别，只是在基类中说明，具体实现交给派生类。

![image-20210415131904033](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232018132.png)

![image-20210415131917288](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232018293.png)

### 1.3应用抽象类建立栈与队列

两种类型的表(list)：栈与队列。

```C++
#include <iostream>
#include <cstdio>
using namespace std;
class list{ // 声明一个抽象类
    public:
        list* head; // 表头指针
        list* tail; // 表尾指针
        list* next;
        int num;
        list(){
            head=tail=next=NULL;
        }
        virtual void store(int i) = 0; // 纯虚函数
        virtual int retrieve() = 0;
};
class queue:public list{
    public:
        void store(int i);
        int retrieve();
};
void queue::store(int i){ // 定义虚函数
    list* item;
    item = new queue;
    if (!item){
        cout << "Allocation error\n";
        exit(1);
    }
    item->num = i;
    if (tail) tail->next = item;
    tail = item;
    item->next = NULL;
    if (!head) head = tail;
}
int queue::retrieve(){
    int i;
    list* p;
    if (!head){
        cout << "list empty\n";
        return 0;
    }
    i = head->num;
    p = head;
    head = head->next;
    delete p;
    return i;
}
class stack:public list{
    public:
        void store(int i);
        int retrieve();
};
void stack::store(int i){
    list* item;
    item = new stack;
    if (!item){
        cout << "Allocation error\n";
        exit(1);
    }
    item->num = i;
    if (head) item->next = head;
    head = item;
    if (!tail) tail = head;
}
int stack::retrieve(){
    int i;
    list* p;
    if (!head){
        cout << "list empty\n";
        return 0;
    }
    i = head->num;
    p = head;
    head = head->next;
    delete p;
    return i;
}
int main(){
    list* p; // 定义指向抽象类list的指针p
    queue q_ob;
    p = &q_ob; // p指向queue对象q_ob
    p->store(1);
    p->store(2);
    p->store(3);
    cout << "queue:";
    for (int i = 0;i < 3;i ++) cout << p->retrieve();
    cout << '\n';
    stack s_ob;
    p = &s_ob;
    p->store(1);
    p->store(2);
    p->store(3);
    cout << "stack:";
    for (int i = 0;i < 3;i ++) cout << p->retrieve();
    cout << '\n';
    return 0;
}
```

