---
layout: post
title: "投稿须知"
sticky: true
date: 2015-04-26 10:07:00
tag: 
- CoderUnion
categories: 公告
---

* content
{:toc}

# 关于投稿

为了促进江南大学技术交流氛围，沉淀技术内容，本站作为一个公共的博客站接受投稿。

## 文章

- 宗旨：引路 + 深入的知识（靠自学）

- 引路：每个目录底下**置顶**xxx（目录名）学习之路的文章

- 深入的知识：推荐，将几个链接放到文章里，作为锦集出现。

# 要投稿需要满足的条件

- 有一个自己的Blog

因为我们鼓励大家相互交流，所以建议大家投稿时**保留原文地址**，我们也鼓励评论时**移步到原文博客讨论**。

- 会使用`Markdown`和[`Jekyll`](http://jekyllcn.com/)

因为本站使用`Jekyll`搭建，所以请至少掌握`Markdown`的基本用法，教程见[Markdown 语法说明](http://wowubuntu.com/markdown/)

- 有一个`Github`账号

本站托管在`Github Pages`上，投稿审核发布的流程也依赖`Github`进行，请注册一个`Github`账号。

# 投稿的流程

## 注意：如果没有时间注册或使用Github走完流程的文章，也可以直接发邮件给simplyyu@163.com，附上文章的地址，由管理员弯成后面的步骤。

## Step 1：开Issue

到我们的[Issues](https://github.com/CoderUnion/coderunion.github.io/issues)开一个Issues，附上文章地址。

![issue](http://i2.tietuku.com/6b0aab5c42d39941.png)

## Step 2：等待管理员处理Issue

管理员给Issue打上相应的label，然后审核原文是否达到要求，并说明理由。达到要求后Pass（同时打上`Pass` label），并由管理员告知需要使用的文件头（`Jekyll`使用）。

![issue pass](http://i1.tietuku.com/dd1d9acb09360773.png)

## Step 3：新建文件，开Pull&Requests

前往[_posts](https://github.com/CoderUnion/coderunion.github.io/tree/master/_posts)直接**新建文件**，选择**create pull requests**

按照要求填好内容后，点击`Propose new file`.(注意文件名用固定格式日期-名称.md，例如`2015-4-26-static-assert-in-c-cpp.md`)

在RP中引用之前开的Issue，并递交。

![new file](http://i2.tietuku.com/34018e60d98e8d39.png)

![propose](http://i2.tietuku.com/8312a4d67f7a0e2c.png)

![pr](http://i2.tietuku.com/2f7020257bcd8c37.png)

## Step 4：管理员处理PR

管理员提出问题后，递交方修改文章，符合要求后merge。

![pr-comment](http://i2.tietuku.com/316c2acdadb4633f.png)

![pr-commit](http://i2.tietuku.com/c5f2e1a42e8f1e24.png)

# 注

- 上面的操作都可以在Github的Web页面中完成，也可以fork clone到本地完成。

- 上面的例子其实遗漏了原文地址的引用，处于方便直接由管理员修改。若出现类似问题也可以要求管理员帮忙修改或者再开一个PR。

- 请不要在自己的递交内修改其他人的文件，避免造成务必要的合并冲突。
