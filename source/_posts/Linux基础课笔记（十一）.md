---
title: Linux基础课笔记（十一）
tags:
  - Linux
  - Thrift
categories:
  - Linux
mathjax: false
copyright: true
abbrlink: b426d4d8
date: 2021-09-21 17:09:47
---

# 1：lesson6

## 1.1：Thrift概述

Apache Thrift官网： https://thrift.apache.org/。

官网教程： https://thrift.apache.org/tutorial/。

官网推荐指南： https://diwakergupta.github.io/thrift-missing-guide/。

学习项目地址： https://git.acwing.com/grant_drew/thrift_lesson。

<!--more-->

C++多线程并发基础入门教程 - zizbee的文章 - 知乎：  https://zhuanlan.zhihu.com/p/194198073。（看这个入门C++多线程）

《C++并发编程实战 第二版》 ： https://www.bookstack.cn/read/CPP-Concurrency-In-Action-2ed-2019/README.md。

---

Thrift 是由 Facebook 开源的轻量级、跨语言 RPC 框架，为数据传输、序列化以及应用级程序处理提供了清晰的抽象和实现。我们可以通过中间语言 IDL 来定义 RPC 接口和数据类型，再通过编译器来生成不同语言对应的代码，最后基于这些自动生成的代码通过相应的编程语言来构建 RPC 客户端和服务端

Apache Thrift 软件框架，用于**可扩展的跨语言服务开发**，将软件堆栈与代码生成引擎相结合，构建在 C++、Java、Python、PHP、Ruby、Erlang、Perl、Haskell、C#、 Cocoa、JavaScript、Node.js、Smalltalk、OCaml 和 Delphi 等语言。

视频教程： https://www.bilibili.com/video/BV1j44y1q7fy。

Thrift非常有用，AcWing的saber和评测等等功能都是由Thrift实现的。

游戏匹配的微服务框架：

![image-20211001084413991](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232129912.png)

Thrift应用于不同节点的进程之间的调用。这其实是一个RPC模型（框架）。

- RPC（Remote Procedure Call）远程过程调用，简单的理解是一个节点请求另一个节点提供的服务

RPC最主要的作用就是用于服务调用。

## 1.2：Thrift实操

本地新建一个仓库用于Thrift学习。

完整项目讲解参考： https://git.acwing.com/fashen/thrift_learning。

```shell
acs@5b4f3438bdf2:~$ mkdir thrift_lesson
acs@5b4f3438bdf2:~$ cd thrift_lesson/
acs@5b4f3438bdf2:~/thrift_lesson$ vim readme.md
acs@5b4f3438bdf2:~/thrift_lesson$ git init
Initialized empty Git repository in /home/acs/thrift_lesson/.git/
acs@5b4f3438bdf2:~/thrift_lesson$ git add .
acs@5b4f3438bdf2:~/thrift_lesson$ git commit -m "init repo"
```

在Gitlab上创建空项目，然后将本地仓库推送到远端。

```shell
acs@5b4f3438bdf2:~/thrift_lesson$ git remote add origin 
acs@5b4f3438bdf2:~/thrift_lesson$ git push -u origin master
```

![image-20211001085820483](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232129916.png)

学习任务：实现“游戏”节点和“匹配系统”节点。

```shell
acs@5b4f3438bdf2:~/thrift_lesson$ mkdir match_system  # 匹配系统文件夹
acs@5b4f3438bdf2:~/thrift_lesson$ mkdir game # 游戏文件夹
acs@5b4f3438bdf2:~/thrift_lesson$ mkdir thrift # 存储thrift接口

acs@5b4f3438bdf2:~/thrift_lesson$ cd thrift/
acs@5b4f3438bdf2:~/thrift_lesson/thrift$ vim match.thrift
```

参照Thrift_lesson仓库下的thrift文件夹下的`example.thrift`编写thrift接口，是官网的一个示例文件。

开始编写`match.thrift`：

```thrift
namespace cpp match_system   # 定义命名空间，match_system是文件名

# 定义结构体，i32表示32位有符号整数，string表示字符串
struct User{
1:i32 id,
      2:string name,
      3:i32 score
}

# 定义service，里面有函数
service Match{
    /**
     * user: 添加的用户信息
     * info: 附加信息
     * 在匹配池中添加一个名用户
     */

    i32 add_user(1:User user,2:string info),  # 添加一个用户
    i32 remove_user(1:User user,2:string info),  # 删除一个用户
}
```

接下来实现匹配系统的服务端（match-server）：C++实现

在本地`match_system`文件夹下新建一个`src`文件夹：

`cd src`执行命令：`thrift -r --gen cpp ../../thrift/match.thrift`，根据thrift文件生成相应cpp代码。

在`src`目录下使用Linux tree命令查看目录内容：

```shell
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ tree  
.
`-- gen-cpp
    |-- Match.cpp
    |-- Match.h
    |-- Match_server.skeleton.cpp
    |-- match_types.cpp
    `-- match_types.h

1 directory, 5 files
```

这些都是Thrift实现好的代码。

```shell
# 将自动生成的gen-cpp文件名修改为match_server
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ mv gen-cpp/ match_server
# 将match_server目录下的xxx.skeleton.cpp拷贝到src目录下命名为main.cpp
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ mv match_server/Match_server.skeleton.cpp main.cpp
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ ls
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ vim main.cpp
```

打开`main.cpp`：（里面的提示都很贴心）

将添加和删除用户的两个函数，补充`return 0;`，保存一下。

然后把cpp文件编译、连接、运行一下，看看是否有问题，没有就开始写代码。

`g++ -c main.cpp match_server/*.cpp`，编译所有cpp文件，不连接。

```shell
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ g++ -c main.cpp match_server/*.cpp
main.cpp:4:10: fatal error: Match.h: No such file or directory
    4 | #include "Match.h"
      |          ^~~~~~~~~
compilation terminated.
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ ls
Match.o  main.cpp  match_server  match_types.o
```

发现有错误需要修改，是头文件路径出错。

看到只有`main.cpp`没有编译生成`.o`文件，打开`mian.cpp`。

头文件路径改成：`"match_server/Match.h"`，然后编译`main.cpp`。

然后链接：将目标文件和库文件整合为可执行文件。

`g++ *.o -o main -lthrift`，后面的`-lthrift`将Thrift的动态链接库整合进来。

就得到了`main`可执行文件，在`main.cpp`中添加提示信息，重新编译、链接、执行（只需要处理`main.cpp`），使用：`./main`运行文件。

修改`main.cpp`：

引入`<iostream>`，`using namespace std;`，在`server.serve();`之前`cout << "start match-server!" << endl;`。

和别人合作最好不要写`using namespace std;`，容易引起命名冲突。

```shell
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ ./main
start match-server!  # 成功，输出提示信息
```

接着持久化仓库，最好把重定向文件和可执行文件删掉，只保留源文件，好习惯！推送到远端。

```shell
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ git add .                                                                                       
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ git status
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ git restore --staged *.o
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ git restore --staged main
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ git status

acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ git commit -m "add main-server"
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ git push
```

接下来实现游戏的客户端（match-client）：python实现

在本地`game`文件夹下新建一个`src`文件夹：

`cd src`执行命令：`thrift -r --gen py ../../thrift/match.thrift`，根据thrift文件生成相应python代码。（python2和python3都可以）

```shell
# 将自动生成的gen-py文件名修改为match_client
acs@5b4f3438bdf2:~/thrift_lesson/game/src$ mv gen-py/ match-client
acs@5b4f3438bdf2:~/thrift_lesson/game/src/match-client$ tree
.
|-- __init__.py
`-- match
    |-- Match-remote
    |-- Match.py
    |-- __init__.py
    |-- constants.py
    `-- ttypes.py

1 directory, 6 files
# Match-remote可执行文件可以直接删掉，用于服务端，这里是客户端
# 中间产生的pyc文件最好也删掉
acs@5b4f3438bdf2:~/thrift_lesson/game/src/match-client$ rm match/Match-remote
```

参照Thrift_lesson仓库下的`game/src/example_client.py`，编写客户端代码。

在`src`下新建`client.py`，把example抄进去，再修改。（`client.py`完整代码见仓库`game/src/`路径）

前四行配置python环境变量直接删掉。

测试一下客户端和服务端的连接是否正常：

先运行服务端：`./main`，然后运行服务端：`python3 client.py`。

![image-20211001113750170](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232129224.png)

改进`client.py`后的测试：

![image-20211001160735297](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232129695.png)

一个Bug复盘：

![image-20211001160827640](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232129086.png)

执行`./main`报错，9090已经占用。

经过一番百度之后，没有结果。

使用`top`命令查看进程情况时，发现有2个tmux在运行（这个好像和会话数量没什么关系，不管了），恍然大悟。然后`tmux ls`查看发现有2个会话正在运行，原来是上午的tmux没关，下午开了一个新的，把上午的会话kill就行了。成功解决Bug。

---

游戏和匹配系统之间构成一个**生产者-消费者模型**，需要用到多线程。

通过**消息队列**来实现生产者与消费者之间的缓冲。

消息队列介绍： https://www.jianshu.com/p/19a94c1c729b。

通过**PV操作互斥信号量**来解决生产者消费者问题。（看看王道视频）

![image-20211023213238739](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232132921.png)

![image-20211002104002433](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232130539.png)

![image-20211002104208317](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232130594.png)

C++多线程笔记： https://github.com/downdemo/Cpp-Concurrency-in-Action-2ed。

![image-20211001163021725](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232132074.png)

改进`main.cpp`：

对于队列的相关操作，应该加锁保证只能有一个线程操作它。

修改完`mian.cpp`之后重新编译、连接、运行：

`g++ -c main.cpp`，`g++ *.o -o main -lthrift -pthread`，连接thrift动态链接库和C++多线程动态链接库。

一个Bug复盘：

链接重定向文件报错，提示未定义引用。

![image-20211002135819071](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232133087.png)

通过`ls`命令查看当前目录内容，发现`src/match_system`下的cpp源文件未编译（之前删除了），

然后把所有源文件重新编译后链接，成功解决。

测试`match-server version2.0`，成功！

![image-20211002140308568](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232133931.png)

---

接下来实现数据保存系统的客户端（save-client）：C++实现

服务端已经实现好了，不需要做了。

编写`save.thrift`：

```thrift
namespace cpp save_service

service Save {

    /**
     * username: myserver的名称
     * password: myserver的密码的md5sum的前8位
     * 用户名密码验证成功会返回0，验证失败会返回1
     * 验证成功后，结果会被保存到myserver:homework/lesson_6/result.txt中
     */
    i32 save_data(1: string username, 2: string password, 3: i32 player1_id, 4: i32 player2_id)
}
// username是homework 4 getinfo的User
// Linux的md5sum命令求出MD5加密后的密码，复制前8位，md5sum，回车，输入密码，回车，ctrl+D
```

`cd ~/thrift_lesson/match_system/src`执行命令：

`thrift -r --gen cpp ../../thrift/save.thrift`，根据thrift文件生成相应cpp代码。

```shell
# 将自动生成的gen-cpp文件名修改为save_client
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ mv gen-cpp/ save_client
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ ls
Match.o  main  main.cpp  main.o  match_server  match_types.o  save_client
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src$ cd save_client/
# 一定要把xxx.skeleton.cpp删掉，不用实现服务端，这里不需要这个文件
acs@5b4f3438bdf2:~/thrift_lesson/match_system/src/save_client$ rm Save_server.skeleton.cpp
```

然后开始完善`main.cpp`：去[官网教程](https://thrift.apache.org/tutorial/cpp.html)看看C++的客户端代码案例，抄过来

写完后保存，回到`src`目录，编译：`g++ -c save_client/*.cpp main.cpp`，链接：`g++ *.o -o main -lthrift -ptherad`。

测试一下：

![image-20211002185857274](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232133713.png)

匹配成功，数据保存成功！

再添加两个用户，发现`result`文件修改为3和4，1和2没有了。

`git add`-->`git commit -m "xxx"`。

---

改进匹配机制，原来的匹配系统只匹配前2个人，现在改进为匹配分差在50以内的2个人。

再次测试3.0，成功！

改进单线程-->多线程，提高效率。

将生产者由单线程扩展成多线程。

再次测试4.0，成功！

最后再来一次优化，加进来的用户，设等待时间为wt秒，就在`[score-wt*50,score+wt*50]`区间内寻找匹配的用户。

再次测试5.0，成功！

![image-20211003104113602](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232133214.png)