---
title: Linux基础课笔记（十）
tags:
  - Linux
categories:
  - Linux
mathjax: true
copyright: true
abbrlink: 907ff002
date: 2021-09-17 15:10:11
---



## 1.4：Git远程仓库

代码托管平台：git.acwing.com，比访问Github更快更方便。

“有了远程仓库，妈妈再也不用担心我的硬盘了。”——Git点读机

<!--more-->

```shell
git remote add origin git@git.acwing.com:xxx/XXX.git：将本地仓库关联到远程仓库
git clone git@git.acwing.com:xxx/XXX.git：将远程仓库XXX下载到当前目录下
```

首先要添加SSH密钥：在本地通过ssh生成一个，然后把公钥粘贴出来到ac git上。

然后就可以新建一个项目，注意如果已经在本地执行过`git init`就**不要再勾选**“使用自述文件初始化仓库”。

```shell
git remote add origin git@git.acwing.com:grant_drew/learn_git.git
# 本地执行，注意在工作目录下执行，将本地仓库关联到ac git上的远程仓库
# 然后将本地仓库推送到ac git上
git push -u origin master # 之后push master就不用加-u了
```

在云端上执行各种操作会更直观，因为是图形化的界面。

“origin” 是当你运行 git clone 时默认的远程仓库名字，相当于一个指向本地仓库的指针。 

在一台新机器执行`git clone`时需要先设置SSH密钥和公钥。（见SSH内容）

**多分支：**（多人协作时用的很多）

分支可以从任何一个节点延伸出来。创建一个分支相当于从这个节点多了一个箭头，但只有commit之后这个箭头才会实际指向一个节点。

注意：**暂存区和分支是相互独立的，多分支公用一个暂存区和工作目录。**当commit时，它会添加到当前分支上。

```shell
git checkout -b branch_name：创建并切换到branch_name这个分支
git branch：查看所有分支和当前所处分支，可选参数-a：列出所有分支，包含远程分支
git checkout branch_name：切换到branch_name这个分支

git merge branch_name：将分支branch_name合并到当前分支上
git branch -d branch_name：删除本地仓库的branch_name分支，删除分支必须要先切换到别的分支
git branch branch_name：创建新分支
！！！要切换分支时，最好先把当前分支add,commit一下
```

git合并操作会直接将HEAD指针的指向修改，而不会将版本复制一份。（要复制的话也可以）

如果想要合并的两个分支可以顺序直接走到，那么会默认为快进（fast-forward），直接修改指针指向。

除此之外，分支合并还有其他两种情况，需要创建新节点，有时候不能合并。（参考[Git官方手册](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6)）

比如，在当前master分支的2号节点下新创建一个分支dev，dev新建一个节点对2号节点的一个文件内容修改，这时回到master分支，也对2号节点内容同一个文件的同一个部分进行修改。那么此时要合并两个分支，会出现冲突，需要手动处理冲突，处理冲突的文件。然后冲突的两个节点和处理完冲突后的节点一起合并到master分支。

```shell
git push -u (第一次需要-u以后不需要)：将当前分支推送到远程仓库
git push origin branch_name：将本地的某个分支推送到远程仓库并合并
git push --set-upstream origin branch_name：设置本地的branch_name分支对应远程仓库的branch_name分支
git push -d origin branch_name：删除远程仓库的branch_name分支
```

---

关于删除分支的问题：

处理本地分支不能完全删除的情况，怎么删除**远程仓库已经删除的分支**呢？

参考资料： https://www.jianshu.com/p/815cf2534362。

假如要删除`dev`分支，在本地切换到`master`分支执行`git branch -d dev`，然后发现还能切换回`dev`分支，很奇怪。很可能是先在远程仓库中删除了该分支，然后在本地删不掉。

接下来先执行`git fetch -p`命令，删除不存在的远程跟踪分支。然后正常切换到`master`分支执行`git branch -d dev`，发现`dev`分支就被彻底删除了。

本地分支可以和云端分支建立映射关系，删除它们中的一个可能另一个还存在，它们是独立的。

为了避免这么麻烦的问题，最后先在本地删除分支，然后`git push -d origin branch_name`删除云端分支。这样就不会有问题。

---

如何将云端分支克隆回本地？

```shell
git branch --set-upstream-to=origin/branch_name1 branch_name2：将远程的branch_name1分支与本地的branch_name2分支对应，先在本地创建一个branch_name2，再将两仓库绑定，然后git pull
git pull：将远程仓库的当前分支与本地仓库的当前分支合并
git pull origin branch_name：将远程仓库的branch_name分支与本地仓库的当前分支合并
```

push和pull是一对命令，推和拉，在云端和本地之间合并分支。

---

Git stash：

```shell
git stash：将工作区和暂存区中尚未提交的修改存入栈中
git stash apply：将栈顶存储的修改恢复到当前分支，但不删除栈顶元素
git stash drop：删除栈顶存储的修改
git stash pop：将栈顶存储的修改恢复到当前分支，同时删除栈顶元素
git stash list：查看栈中所有元素
```

---

云端分支与云端分支合并：

在ac git找到合并请求，选择源分支和目标分支后合并，可以选择合并后删除源分支。

如果权限不够，合并分支可能需要leader审核。

---

仓库的删除：

删除本地仓库：`rm  project -rf`，删除云端仓库：设置-->高级-->删除项目。

## 1.5：acwing讲义

注意：本次作业的10个题目不是独立的，每个题目会依赖于前一个题目，因此评测时如果当前题目错误，则不再评测后续题目。

创建好作业后，先进入文件夹/home/acs/homework/lesson_5/，然后：
    (0) 在当前目录下创建文件夹homework，并将homework目录配置成git仓库。后续作业均在homework目录下操作；
    (1) 创建文件readme.txt，内容包含一行：111；
        将修改提交一个commit；
    (2) 在readme.txt文件末尾新增一行：222；
        将修改提交一个commit；
    (3) 创建文件夹：problem1和problem2；
        创建文件problem1/main.cpp。文件内容为下述链接中的代码：https://www.acwing.com/problem/content/submission/code_detail/7834813/；
        创建文件problem2/main.cpp。文件内容为下述链接中的代码：https://www.acwing.com/problem/content/submission/code_detail/7834819/；
        将修改提交一个commit；
    (4) 删除文件夹problem2；
        创建文件夹problem3；
        创建文件problem3/main.cpp。文件内为下述链接中的代码：https://www.acwing.com/problem/content/submission/code_detail/7834841/；
        将readme.txt中最后一行222删掉，然后添加一行333；
        将修改提交一个commit；
    (5) 在https://git.acwing.com/上注册账号并创建仓库，仓库名称为homework；
        将本地git仓库上传到AC Git云端仓库；
    (6) 创建并切换至新分支dev；
        在readme.txt文件中添加一行444；
        将修改提交一个commit；
        将dev分支推送至AC Git远程仓库；
    (7) 切换回master分支；
        在readme.txt文件中添加一行555；
        将修改提交一个commit；
    (8) 将dev分支合并到master分支；
        手动处理冲突，使readme文件最终内容包含4行：111、333、555、444；
        将修改提交一个commit；
    (9) 将master分支的版本库push到AC Git云端仓库；
        登录myserver服务器（4. ssh作业配置的服务器）；
        创建并清空文件夹：~/homework/lesson_5/；
        将AC Git云端仓库clone到~/homework/lesson_5/中；

---

大部分都很简单，有两个题要注意一下。

```shell
# homework_3
# tmux 里复制会丢失 #i，要手动补全
```

```shell
# homework_9
# 参考题解： https://www.acwing.com/activity/content/code/content/1805869/

# 如果git clone需要密码，可能是myserver服务器没有配置SSH密钥，需要生成一下公钥添加到git lab上
如果gitlab上没有添加myserver服务器上密钥，需要添加密钥。

# 解决git clone只有master(或者其他分支)分支的问题
/*
我们在使用 git clone + 远程仓库地址将项目下载下来之后，倘若远程仓库有多个分支，我们会发现，使用git branch查看本地分支时，只有一个master分支。*/

但是，大多时候我们是需要在其他分支进行工作的，所以我们需要将远程的其他分支拉下来才可以，首先，先使用git branch -a查看远程分支,那些以rmotes开头且为红色的都是远程分支，或者理解为这些分支是隐藏的。*/

如果想要将远程分支与本地分支联系起来，则执行
git checkout -b master origin/master
或者使用-t参数，它默认会在本地建立一个和远程分支名字一样的分支
git checkout -t origin/master
```

