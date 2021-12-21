---
title: Latex常用语法总结
tags:
  - Latex
categories:
  - Latex
mathjax: true
copyright: true
abbrlink: fcaedc74
date: 2021-02-23 23:12:43
---

文章开头加上一句：`mathjax: true`，即可启动Latex渲染。

<!--more-->

推荐一个Latex工具网站： https://latexlive.com/。

1.打出斜体：$O(n)$,`$O(n)$`

2.打出根号：$\sqrt n$ ,`$\sqrt n$`

3.打出上标（单字符）：A$^2$,`A$^2$`

4.打出上标（多字符）：A$^{mod}$,`A$^{mod}$`

5.打出下标（单字符）：A$_2$,`A$_2$`

6.打出下标（多字符）：A$_{mod}$,`A$_{mod}$`

7.打出log对数：$\log_ax$，`$\log_ax$`;	$\ln x$，`\ln x$`;	$\lg x$,`$\lg x$`

8.打出正上方的上标：$\bar a$,`$\bar a$`

9.小写希腊字母：

![image-20210310195229711](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232127919.png)

10.大写希腊字母：

![image-20210310195310812](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232127648.png)

11.打出关于x的偏微分：$\partial/\partial x$,`$\partial/\partial x$`，补充一点：\partial可以简写为\part

12.打出除法：$\frac a b$,`$\frac a b$`

13.求和和连乘

- 对于连加的情况，我们通常使用$\Sigma$来表示。它的使用用法也很简单，但是通常都要添加上下标，像`$\sum_{}^{}$`形式。除了连加，我们有时也使用连乘，虽然没有连加使用得多（连乘都能通过对数写成连加），它只要以`$\prod_{}^{}$`的形式表示。

```Latex
<!--连加-->
$$\sum_{i = 1}^{n}x_i$$

<!--连乘-->
$$\prod_{i = 1}^{n}x_i$$
```

- 在latex中，默认情况下行内公式都是显示像$\sum_{i = 1}^{n}x_i$的效果，如果想要这样$\displaystyle\sum_{i = 1}^{n}x_i$的效果，就需要在前面加上`\displaystyle`，来重新看一下下面的例子：

```latex
<!--连加-->
$\sum_{i = 1}^{n}x_i$
$\displaystyle\sum_{i = 1}^{n}x_i$

<!--连乘-->
$\prod_{i = 1}^{n}x_i$
$\displaystyle\prod_{i = 1}^{n}x_i$
```

补充：关于上下标的位置有以下三种情况

`\sum\nolimits_{j=1}^{M}` 上下标位于求和符号的水平右端，

`\sum\limits_{j=1}^{M}` 上下标位于求和符号的上下处，

`\sum_{j=1}^{M}` 对上下标位置没有设定，会随公式所处环境自动调整。

14.无穷大：$\infty$,`$\infty$`

正无穷大： +\infty

负无穷大： -\infty

15.极限：

还记得高数里极限的符号吗![皱眉](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232127965.png)。在latex中的极限表示，也直接使用`\lim`这个我们时常看到的符号。当然极限通常都是带下标的，所以更多的是使用`lim_{}`的形式。

```latex
<!--来看看两个重要极限-->
$$\displaystyle\lim_{x \rightarrow 0}\frac{\sin x}{x} = 1$$

$$\displaystyle\lim_{x \rightarrow + \infty}(1 + \frac{1}{x})^x = e$$
```

效果如下：

$\displaystyle\lim_{x \rightarrow 0}\frac{\sin x}{x} = 1$

$\displaystyle\lim_{x \rightarrow + \infty}(1 + \frac{1}{x})^x = e$

- **tip1：**右箭头$\rightarrow$的表示方式为`$\rightarrow$`，左箭头$\leftarrow$的表示方式是`$\leftarrow$`
- **tip2：**正无穷$+ \infty$的表示方式为`$+ \infty$`，负无穷$- \infty$的表示方式是`$- \infty$`

16.积分：

如果想要输入积分，则需要使用`\int_{}^{}`来表示

```latex
$$\int_0^1 x^2 dx$$
<!--来看一个更加复杂的例子-->
<!--正态分布的分布函数-->
$$F(x) = \int_{- \infty}^{+ \infty} \frac{1}{\sqrt{2\pi}\sigma}e^{-(\frac{x-\mu}{\sigma})^2} dx$$
```

效果如下：

$\int_0^1 x^2 dx$

$F(x) = \int_{- \infty}^{+ \infty} \frac{1}{\sqrt{2\pi}\sigma}e^{-(\frac{x-\mu}{\sigma})^2} dx$

17.求导：

使用`$\mathrm{d}$`来表示求导符号，`$\partial$`来表示求偏导

```latex
$\frac {\mathrm{d}L(\beta)}{\beta}$

<!--直接用d来表示求导符的效果-->
$\frac {dL(\beta)}{\beta}$

<!--偏导-->
$\frac {\partial L(\beta_0, \beta_1)}{\partial \beta_0}$
```

效果如下：

$\frac {\mathrm{d}L(\beta)}{\beta}$

$\frac {dL(\beta)}{\beta}$

$\frac {\partial L(\beta_0, \beta_1)}{\partial \beta_0}$

18.打出空格：$a \ b$,`$a \ b$`

19.打出向上向下取整符号：

上取整$\lceil x \rceil$(表示不小于x的最小整数),`$\lceil x \rceil$`

下取整$\lfloor x \rfloor$(表示不大于x的最大整数),`$\lfloor x \rfloor$`

20.打出实数集符号：$\mathbb{R}$，`$\mathbb{R}$`;$\mathbb{R}^{n}$,`$\mathbb{R}^{3}$`

21.打出换行符号（PS：空格或enter是没用的）：$a\\b$，`$a\\b$`

22.方程组的排版：https://blog.csdn.net/weixin_45744426/article/details/102531827

一个示范：$\begin{array}{l}x=x_0+k*b^{'}\\ y=y_0-k*a^{'}\end{array}$，无花括号版本的，begin的参数可以选择l、r和c，表示左、右和居中对齐。

23.打出恒等号：$\equiv$，`$\equiv$`

24.打出空格：$a\;b$，`$a\;b$`

25.打出整除与不整除：$a\mid b,c\nmid d$，`$a\mid b,c\nmid d$`

26.Latex公式渲染问题，两个`*`会被hexo转义为斜体，所以用`\star`代替。