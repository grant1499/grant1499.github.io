---
title: Ubuntu20系统下使用vimplus教程
tags:
  - Linux
categories:
  - Linux
mathjax: true
copyright: true
abbrlink: bb6f50f1
date: 2021-08-27 14:01:52
---

# VimPlus : An automatic configuration program for vim

对于想用vim又觉得vim的配置很麻烦的人来说，vimplus绝对是一个很棒的工具！

参考资料： [vimplus官方Github地址](https://github.com/chxuan/vimplus)。

<!--more-->

在Ubuntu20系统下安装vimplus：

```shell
git clone https://github.com/chxuan/vimplus.git ~/.vimplus
cd ~/.vimplus
./install.sh //不加sudo
```

tips：你需要先安装git。

设置Nerd Font：

为防止vimplus显示乱码，需设置linux终端字体为`Droid Sans Mono Nerd Font`。

安装vimplus的过程中可能出现**各种状况**：

请参考b站视频教程： https://www.bilibili.com/video/BV1rA41157qK?p=2。

```C++
视频中的配置步骤 及其相关的下载安装包和插件包
链接：https://pan.baidu.com/s/1Vy1gRltUIyG-y0X_doGMhw 
提取码：6666 
```

最后安装完后会自动进入插件的安装 , 最后会进入一个各种插件的下载界面, 如果此时插件正常下载可以等待下载完成.
如果插件下载半天没动静, 或者很多插件都下载不成功,可以参考视频中,也就算下面的方法进行设置.

```C++
最后你的vim的所有插件都放在 ~/.vim/plugged目录下, 可以进入 cd ~/.vim/plugged 进行查看.
如果发现插件只有一个Ycm或者只有很少的插件, 那么可以拷贝我的plugged.zip文件到你的linux上, 然后unzip plugged.zip
得到一个plugged目录, 然后我们将原本的plugged删除, rm -rf ~/.vim/plugged , 将我们解压得到的plugged移动过去
mv plugged ~/.vim , 最后再写入我们的配置文件, 这里可以直接用我的vimrcbf, 将此时的.vimrc删除或者备份一份也可. 
rm -f ~/.vimrc, 其实这个.vimrc是个软链接. 然后将vimrcbf 改名为 .vimrc. 
mv vimrcbf .vimrc  
然后再试试vim是否已经具备功能了
```

进入vim之后在normal模式下, 按下 <leader> + h ， 即可打开帮助文档. <leader>默认为"," 可以在.vimrc进行修改, 很多功能的快捷键均可在.vimrc进行修改.

打开vim时可能会有ycm插件提示的shutdown等错误信息，此时需要切换到.vim/plugged/YouCompleteMe目录下，执行`python3 install.py`。

```shell
cd ~/.vim/plugged/youcompleteme
python3 install.py
```

