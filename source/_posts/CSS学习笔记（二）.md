---
title: CSS学习笔记（二）
tags:
  - CSS
categories:
  - - CSS
mathjax: true
copyright: true
abbrlink: b173af83
date: 2021-05-10 20:58:29
---

> 续CSS学习笔记（一）。

### 4.2高级选择器（现查现用）

#### 4.2.1层次选择器

先设计网页框架。

<!--more-->

![image-20210510210715099](CSS学习笔记（二）/image-20210510210715099.png)

1. 后代选择器：在某个元素的后面   祖父  父亲 儿子  孙子

    ```css
    body p{
        background: #24c497;
    }
    ```

2. 子选择器：只有一代，儿子

    ```css
    body>p{
        background: #f39cff;
        /*p1 p2 p3变色*/
    }
    ```

3. 相邻兄弟选择器：同辈
    
只有一个相邻兄弟，向下
    
    ”弟弟“选择器
    
    ```css
    .active+p{
        background: #f39cff;
        /*p2会变色*/
    }
<p class="active">p1</p>
    <p>p2</p>
    ```
    
4. 通用”弟弟“选择器

    当前选中元素向下的所有兄弟元素

    ```css
    .active~p{
        background: #f39cff;
        /*p2 p3 p7 p8变色*/
    }
    ```

    ---

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <style>
            /*后代选择器*/
            /*body p{*/
            /*    background: #24c497;*/
            /*}*/
            /*子选择器*/
            /*body>p{*/
            /*    background: #f39cff;*/
            /*}*/
            /*兄弟选择器
            只有一个，向下
            */
            /*.active+p{*/
            /*    background: #f39cff;*/
            /*}*/
            /*通用选择器*/
            .active~p{
                background: #f39cff;
            }
        </style>
    </head>
    <body>
        <p class="active">p1</p>
        <p>p2</p>
        <p>p3</p>
        <ul>
            <li>
                <p>p4</p>
            </li>
            <li>
                <p>p5</p>
            </li>
            <li>
                <p>p6</p>
            </li>
        </ul>
        <p>p7</p>
        <p>p8</p>
    </body>
    </html>
    ```