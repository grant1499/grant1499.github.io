---
title: Hello World
copyright: true
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

<!--more-->

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)

### hexo相关问题汇总

1.只需在 md 文件里使用 `![title](同名文件夹/图片名.jpg)` ，无需路径名就可以插入图片。

2.在文章`<!--more-->`之前的内容将作为首页的摘要部分！

3.[解决busuanzi不显示问题必看](https://muyuuuu.github.io/2019/11/22/busuanzi-notdisplay/)

其实核心问题就是找到span id里是否有style = "display: none"，找到对应文件对应位置的代码并删除即可

4.hexo s本地样式与网页样式不一致？

- 其实原因很简单，拉到服务器之前要`hexo clean`清理一下缓存，然后再生成和部署，你学会了吗？


（可能`hexo c && hexo g && hexo d`一步到位，有时候不太行，分开写）

- 或者清理一下浏览器缓存就可以解决。

5.[markdown语法速成宝典](https://www.kuangstudy.com/bbs/1356475333565587458)

6.推荐hexo搭建文章：https://tding.top/archives/aad98408.html