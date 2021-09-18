---
title: Linux基础课笔记（八）
tags:
  - Linux
categories:
  - Linux
mathjax: true
copyright: true
abbrlink: 6f80f499
date: 2021-09-05 15:57:26
---

# 1：lesson4

## 1.1：SSH概述

SSH参考教程： https://wangdoc.com/ssh/。（很棒）

SSH 是 Linux 系统的登录工具，现在广泛用于服务器登录和各种加密通信。

<!--more-->

SSH（Secure Shell 的缩写）是一种网络协议，用于加密两台计算机之间的通信，并且支持各种身份验证机制。

实务中，它主要用于保证远程登录和远程通信的安全，任何网络服务都可以用这个协议来加密。

## 1.2：SSH登录

远程登录服务器：`ssh user@hostname`

- user: 用户名
- hostname: IP地址或域名

第一次登录时会提示：这是一个验证过程。

```shell
The authenticity of host '123.57.47.211 (123.57.47.211)' can't be established.
ECDSA key fingerprint is SHA256:iy237yysfCe013/l+kpDGfEG9xxHxm0dnxnAbJTPpG8.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

输入yes，然后回车即可。

这样会将该服务器的信息记录在~/.ssh/known_hosts文件中。

然后输入密码即可登录到远程服务器中。

ssh指定连接端口：

ssh 默认连接服务器的22端口，`-p`参数可以指定其他端口。

输入`exit`命令或者`ctrl+d`退出ssh登录。

## 1.3：客户端配置文件

SSH 客户端的全局配置文件是`/etc/ssh/ssh_config`，用户个人的配置文件在`~/.ssh/config`，优先级高于全局配置文件。

**特别注意：这是在客户端的配置文件，不要进入服务器配置了。**

除了配置文件，`~/.ssh`目录还有一些用户个人的密钥文件和其他文件。

创建用户个人配置文件 `~/.ssh/config`，可以按照不同服务器，列出各自的连接参数，从而不必每一次登录都输入重复的参数。

```shell
~/.ssh/config配置文件演示
------------------------
Host *
     Port 2222

Host remoteserver
     HostName remote.example.com # 注意单词大小写
     User neo
     Port 2112
```

上面代码中，`Host *`表示对所有主机生效，后面的`Port 2222`表示所有主机的默认连接端口都是2222，这样就不用在登录时特别指定端口了。这里的缩进并不是必需的，只是为了视觉上，易于识别针对不同主机的设置。

后面的`Host remoteserver`表示，下面的设置只对主机`remoteserver`生效。**`remoteserver`只是一个别名**，具体的主机由`HostName`命令指定，**`User`和`Port`这两项分别表示用户名和端口**。这里的`Port`会覆盖上面`Host *`部分的`Port`设置。**`HostName`后面可以接IP地址或域名。之后再使用服务器时，可以直接使用别名。**

以后，登录`remote.example.com`时，只要执行`ssh remoteserver`命令，就会自动套用 config 文件里面指定的参数。

```shell
$ ssh remoteserver
# 等同于
$ ssh -p 2112 neo@remote.example.com
```

`Host`命令的值可以使用通配符，比如`Host *`表示对所有主机都有效的设置，`Host *.edu`表示只对一级域名为`.edu`的主机有效的设置。它们的设置都可以被单个主机的设置覆盖。

## 1.4：SSH密钥登录

SSH 默认采用密码登录，这种方法有很多缺点，简单的密码不安全，复杂的密码不容易记忆，每次手动输入也很麻烦。密钥登录是比密码登录更好的解决方案。

客户端创建密钥：`ssh-keygen`，然后一直回车就行。注意在客户端执行。

执行结束后，`~/.ssh/`目录下会多两个文件：

1. `id_rsa`：私钥（不能给别人看的，如果暴露了最好重新生成一遍）
2. `id_rsa.pub`：公钥（可以公开的），相互配对的

之后想免密码登录哪个服务器，就将公钥传给哪个服务器即可。

例如，想免密登录`myserver`服务器。则将**公钥中的内容**，复制到`myserver`中的`~/.ssh/authorized_keys`文件里即可。多个公钥的话用回车隔开。

也可以使用如下命令一键添加公钥：`ssh-copy-id myserver`（等价于将本地公钥复制到服务器上）

## 1.5：SSH执行远程命令

当你有很多台服务器时，可以通过执行远程命令完成自动化操作，不需要一个个登录ssh。

```shell
# 命令格式：
ssh user@hostname command
# 例如：
ssh user@hostname ls -a
```

```shell
# 单引号中的$i可以求值
ssh myserver 'for ((i = 0; i < 10; i ++ )) do echo $i; done'
# 双引号中的$i不可以求值
ssh myserver "for ((i = 0; i < 10; i ++ )) do echo $i; done"
```

命令执行的输出会重定向（传回）到本地终端显示输出。

举个例子：

```shell
a=1
ssh myserver  "echo $a"
ssh myserver  'echo $a'
```

双引号是在本地服务器进行转义了，所以传过去命令不是`echo $a`，而是`echo 1`;单引号传过去的是`echo $a`。

## 1.6：scp命令传文件

`scp`是 SSH 提供的一个**客户端**程序，用来在两台主机之间加密传送文件（即复制文件）。

`scp`是 secure copy 的缩写，相当于`cp`命令 + SSH。它的底层是 SSH 协议，默认端口是22，相当于先使用`ssh`命令登录远程主机，然后再执行拷贝操作。

`scp`主要用于以下三种复制操作。

- 本地复制到远程。
- 远程复制到本地。
- 两个远程系统之间的复制。（可以直接传，但要完整授权，可以用服务器1-->本地-->服务器2代替）

使用`scp`传输数据时，文件和密码都是加密的，不会泄漏敏感信息。

`scp`的语法类似`cp`的语法。

```shell
scp source destination
```

将source路径下的文件复制到destination中。

可以一次复制多个文件：

```shell
scp source1 source2 destination
```

注意，如果所要复制的文件，在目标位置已经存在同名文件，`scp`会在没有警告的情况下覆盖同名文件。

**复制文件夹：**（加一个参数`-r`）

`scp -r ~/tmp myserver:/home/acs/`

将本地家目录中的tmp文件夹复制到myserver服务器中的/home/acs/目录下。

`scp -r ~/tmp myserver:homework/`

将本地家目录中的tmp文件夹复制到myserver服务器中的~/homework/目录下。

`scp -r myserver:homework .`

将myserver服务器中的~/homework/文件夹复制到本地的当前路径下。

可以看到，主机与文件之间要使用冒号（`:`）分隔。后面如果是相对路径是相对ssh连接User的家目录。

**指定服务器的端口号：**

`scp -P 22 source1 source2 destination`

**注意： scp的-r -P等参数尽量加在source和destination之前。**（-P是大写）

使用scp配置其他服务器的vim和tmux：（装修毛胚服务器~）

`scp ~/.vimrc ~/.tmux.conf myserver:`

## 1.7：注意

看清楚，以上命令或配置大多在**客户端（本地）**执行，不要在服务器上执行！！！

## 1.8：作业

4. 作业
    创建好作业后，先进入文件夹/home/acs/homework/lesson_4/，然后：
    (0) 进入homework_0文件夹，要求：
        [1] 该文件夹内容为空
        [2] 配置服务器账号的密钥登陆方式。服务器信息可以通过如下命令获得：
            homework 4 getinfo
            将服务器账号的名称（Host）配置成：myserver
    (1) 进入homework_1文件夹，下列描述中的“本地”均表示当前文件夹。要求：
        [1] 在myserver服务器上创建并清空文件夹：~/homework/lesson_4/homework_1/
        [2] 将本地的main.cpp文件上传到myserver中的~/homework/lesson_4/homework_1/目录中。
        [3] 在本地创建文件夹dir。
        [4] 将myserver中的/etc/lsb-release文件复制到dir中。
    (2) 进入homework_2文件夹，下列描述中的“本地”均表示当前文件夹，要求：
        [1] 在myserver服务器上创建并清空文件夹：~/homework/lesson_4/homework_2/
        [2] 将本地的dir文件夹上传到myserver中的~/homework/lesson_4/homework_2/目录中。
    (3) 进入homework_3文件夹，下列描述中的“本地”均表示当前文件夹，要求：
        [1] 在本地创建文件夹dir。
        [2] 将myserver中的/var/lib/locales/supported.d文件夹下载到本地的dir文件夹中。
    (4) 进入homework_4文件夹，编写脚本remote_mkdir.sh和remote_rmdir.sh，要求：
        [1] 在myserver服务器上创建并清空文件夹：~/homework/lesson_4/homework_4/
        [2] 本地目录下仅包含remote_mkdir.sh和remote_rmdir.sh
        [3] remote_mkdir.sh和remote_rmdir.sh具有可执行权限
        [4] remote_mkdir.sh接收一个传入参数。格式为 ./remote_mkdir.sh directory_name
            该操作可以在myserver服务器上的~/homework/lesson_4/homework_4/目录下，创建一个名为directory_name的文件夹
        [5] remote_rmdir.sh接收一个传入传输。格式为 ./remote_rmdir.sh directory_name
            该操作可以将myserver服务器上的~/homework/lesson_4/homework_4/目录下的名为directory_name的文件夹删掉。
        [6] 注意：传入的文件参数可能包含空格。两个脚本均不需要判断传入参数的合法性。

```shell
# 0
# 基本配置，讲义中有

# 1
ssh myserver 'mkdir -p ~/homework/lesson_4/homework_1/'
scp main.cpp myserver:~/homework/lesson_4/homework_1
mkdir dir
scp myserver:/etc/lsb-release ./dir

# 2
ssh myserver 'mkdir -p ~/homework/lesson_4/homework_2/' #复习创建多级目录使用-p
scp -r dir myserver:~/homework/lesson_4/homework_2/ #传文件用scp，别再ssh

# 3
mkdir dir
scp -r myserver:/var/lib/locales/supported.d ./dir/

# 4
# 嵌套引号，外层引号优先级更高，shell中双引号在外可以解析内层单引号内部的变量值
# 内层 $1 加单引号可以防止文件名有空格
# mkdir：
#! /bin/bash

dir=homework/lesson_4/homework_4
ssh myserver mkdir "${dir}/'$1'"
# rmdir：
#! /bin/bash

dir=homework/lesson_4/homework_4
ssh myserver rm -r "${dir}/'$1'"
# 注意：若文件、文件夹名字中有空格，系统会用单引号包裹避免歧义，使用的时候也加上单引号
```

