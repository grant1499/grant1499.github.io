---
title: Linux基础课笔记（九）
tags:
  - Linux
  - Git
categories:
  - Linux
mathjax: true
copyright: true
abbrlink: 154dd5c6
date: 2021-09-14 21:46:29
---

# 1：lesson5

## 1.1：Git概述

<!--more-->

git官方中文教程： https://git-scm.com/book/zh/v2。

廖雪峰教程： https://www.liaoxuefeng.com/wiki/896043488029600。

git参考手册： http://gitref.justjavac.com/index.html。

交互式学习git网站： https://learngitbranching.js.org/?locale=zh_CN。

把git掌握好对于以后工作有很大帮助的。

---

Git是什么？

Git是目前世界上最先进的**分布式**版本控制系统（没有之一）。

Git有什么特点？简单来说就是：高端大气上档次！

> 学git最好要像学kmp，尽量第一遍彻底搞明白，不要像学vim,tmux一样现查命令。——y总

Git以一棵树（有向树）的形式来管理不同的版本，每个版本就是树的一个节点，支持多分支。

## 1.2：Git基本概念

参考文章： https://www.liaoxuefeng.com/wiki/896043488029600/897271968352576。

1. 工作区（Working Directory）：仓库的目录，就是你本地的文件夹。工作区是独立于各个分支的。
2. 暂存区（Stage）：数据暂时存放的区域，类似于工作区写入版本库前的缓存区。暂存区是独立于各个分支的。
3. 版本库（Repository）：版本库也就是仓库，存放所有已经提交到本地仓库的代码版本。
4. 版本结构：树结构，树中每个节点代表一个代码版本。

## 1.3：Git常用命令

`Git config`配置：

在git中，我们使用git config 命令用来配置git的配置文件，git配置级别主要有以下3类：

1、仓库级别 local 【优先级最高】

2、用户级别 global【优先级次之】

3、系统级别 system【优先级最低】

通常：git 仓库级别对应的配置文件是当前仓库下的.git/config，

git 用户级别对应的配置文件是用户宿主目录下的~/.gitconfig，

git系统级别对应的配置文件是git安装目录下的 /etc/gitconfig。

对于git来说，配置文件的权重是仓库>全局>系统。

```shell
git config -e 编辑配置文件 
	git config --local -e 编辑仓库级别配置文件
	git config --global -e 编辑用户级别配置文件
	git config --system -e 编辑系统级别配置文件
	
git config --global user.name xxx：设置全局用户名，信息记录在~/.gitconfig文件中
git config --global user.email xxx@xxx.com：设置全局邮箱地址，信息记录在~/.gitconfig文件中
```

创建一个版本库（仓库）：

首先，选择一个合适的地方，创建一个空目录，

第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：

`git init`：将当前目录配置成git仓库，信息记录在隐藏的.git文件夹中。

```shell
acs@6961ddc3b91b:~$ mkdir project
acs@6961ddc3b91b:~$ cd project/
acs@6961ddc3b91b:~/project$ git init
Initialized empty Git repository in /home/acs/project/.git/
```

可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

---

现在我们编写一个`readme.txt`文件，一定要放到工作区目录下（子目录也行），不然Git找不到。

接下来就是把这个文件放到Git仓库：

第一步，用命令`git add`告诉Git，把文件添加到仓库：`$ git add readme.txt`，

执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。

```shell
git add XX：将XX文件添加到暂存区，也可以表示将文件的删除、修改状态加入暂存区
	git add .：将所有待加入暂存区的文件加入暂存区

git status：查看仓库状态
```

添加文件前后可以使用`git status`命令来查看仓库发生了什么变化。

**`git status`命令还能够提醒你可能会用到什么命令，非常智能。**

第二步，用命令`git commit`告诉Git，把文件提交到仓库：

```shell
git commit -m "给自己看的备注信息"：将暂存区的内容提交到当前分支
```

备注信息可以是任何东西，最好有意义。m的意思是message。

```shell
acs@6961ddc3b91b:~/project$ git commit -m "wrote a readme file"
[master (root-commit) 410ff0b] wrote a readme file
 1 file changed, 1 insertion(+)
 create mode 100644 readme.txt
```

为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件。

注意：执行`git commit`之后，暂存区的内容会清空。

`git commit`不一定会将所有文件修改都提交到仓库，如果一部分文件没有加入暂存区，那就不会被提交。

```shell
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```

---

修改`readme.txt`文件并重新提交：

```shell
git diff XX：查看XX文件相对于暂存区修改了哪些内容，diff也就是difference
```

我们先用vim修改了`readme.txt`，然后使用：`git diff readme.txt`。

```shell
acs@6961ddc3b91b:~/project$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index d6d92b1..a972df7 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1 +1,3 @@
 hello,git is a version control system!
+
+Java is a great program language!
```

`git diff`比较的是工作区中实际的文件与暂存区中的文件，暂存区存的是上一次`git add`时的文件，所以不一样。

知道了对`readme.txt`作了什么修改后，再把它提交到仓库就放心多了，提交修改和提交新文件是一样的两步，第一步是`git add`，第二步是`git commit`。

```shell
acs@6961ddc3b91b:~/project$ git add readme.txt 
acs@6961ddc3b91b:~/project$ git commit -m "readme file changed"
[master ad2e3d2] readme file changed
 1 file changed, 2 insertions(+)
acs@6961ddc3b91b:~/project$ git status
On branch master
nothing to commit, working tree clean
```

![image-20210917093619626](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232128492.png)

**Git删除文件命令：**

`git rm` ： **同时从工作区和索引中删除文件**。即本地的文件也被删除了。

`git rm --cached` ： **从索引中删除文件。但是本地文件还存在**， 只是不希望这个文件被版本控制。

```shell
git rm --cached XX：将文件从仓库索引目录中删掉，还可以git add回到暂存区来
```

`git rm`与`rm`的区别：

`git rm`： 来删除文件，同时还会将这个删除操作记录下来（也就是将操作add到暂存区）;

`rm`： 来删除文件，仅仅是删除了物理文件，没有将其从 git 的记录中剔除;

提示：即使你已经通过 rm 将某个文件删除掉了，也可以再通过 `git rm` 命令重新将该文件从 git 的记录中删除掉。

注意：上述操作最后要执行`git commit`才真正提交到git仓库。

**Git撤销文件命令：**（看y总视频35:40开始，讲得很清楚）

```shell
git restore --staged <file>：作用是将暂存区的文件从暂存区撤出，但不会更改文件，这条命令执行前后file的内容是一致的

git checkout — XX或git restore XX：将XX文件尚未加入暂存区的修改全部撤销
git restore <file>：表示撤销暂存区的修改，将文件状态恢复到add之前的状态
对于已经add过的文件，修改文件之后使用git restore撤销修改！
```

要撤销对工作区（从未加入暂存区）的文件的修改，保险做法是建立仓库时就把所有文件都add进来，可用`git add .`命令，然后撤销文件修改就用`git restore <file>`命令。

```shell
1.建立readme.txt --> 111
2.git add readme.txt
3.修改vim readme.txt --> 111\n222
4.git add readme.txt
5.修改vim readme.txt --> 111\n222\n333
6.git restore readme.txt（回滚到第3步的状态：111\n222）
7.git restore --staged readme.txt（暂存区文件撤出，111\n222）
8.git restore readme.txt（暂存区已空，恢复至HEAD指向当前版本readme.txt，111）
# 7,8这两步操作一起使用是常见的操作
```

**Git版本相关命令：**

```shell
git log：查看当前分支的所有版本
git reflog：查看HEAD指针的移动历史（包括被回滚的版本），最左边是命令的id（版本号）
```

`git log`命令显示从最近到最远的提交日志。

```shell
acs@6961ddc3b91b:~/project$ git log
commit a1538706221fdabbe2d1059dc25e3713c6db387b (HEAD -> master)
                  ...
commit 410ff0b36a7fcff7892814ba31da9335ad445a61
Author: grant <2478881580@qq.com>
Date:   Fri Sep 17 09:18:18 2021 +0800

    wrote a readme file
如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数：
acs@6961ddc3b91b:~/project$ git log --pretty=oneline
a1538706221fdabbe2d1059dc25e3713c6db387b (HEAD -> master) hello
ad2e3d29653d53868f4e41ba9d4219f72a3e82a8 readme file changed
410ff0b36a7fcff7892814ba31da9335ad445a61 wrote a readme file
```

在Git中，用`HEAD`（相当于链表中的一个指针）表示当前版本。

版本回滚命令：

```shell
git reset --hard HEAD^ 或 git reset --hard HEAD~：将代码库回滚到上一个版本
    git reset --hard HEAD^^：往上回滚两次，以此类推
    git reset --hard HEAD~100：往上回滚100个版本
    git reset --hard 版本号：回滚到某一特定版本
```

总结：

- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

