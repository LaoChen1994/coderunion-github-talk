---
layout: post
title: "图像处理的融合算法及在视觉艺术的应用(转)"
date: 2015-04-24 15:55
sticky: false
tag: 
- 图像识别
categories: 机器学习
---

* content
{:toc}

## 图像处理的融合算法及在视觉艺术的应用(转)
原文链接 ：[http://weibo.com/p/1001603838401345359624](http://weibo.com/p/1001603838401345359624)

### 1.基于泊松方程的图像融合方法，利用偏微分方程实现了不同图像上区域的无缝融合。比较经典的文章：
P. Pérez, M. Gangnet, A. Blake. Poisson image editing. ACM Transactions on Graphics (SIGGRAPH'03), 22(3):313-318, 2003.
 下载地址（paper+matlab代码）：
  http://pan.baidu.com/s/1sjt51u9

![](http://ww3.sinaimg.cn/large/0060jr72gw1erqytcl86ej30k20cbgp3.jpg)

### 2.泊松融合的一个基本介绍
[http://blog.sina.com.cn/s/blog_4c191f370100uqro.html](http://blog.sina.com.cn/s/blog_4c191f370100uqro.html)
     下图是图像编辑的结果，通过改变花朵的融合边界，将其重新融合到图像中，从而可以自然地改变其色调，呈现出新的视觉效果。
 
   ![](http://ww1.sinaimg.cn/large/0060jr72gw1erqyqldgayj30c2083753.jpg)
 
### 3：opencv3.0 photo 模块加入了seamless_cloning类
该类对应的论文是“Poisson Image Editing”
    博文(OpenCV)：
    [http://blog.csdn.net/vsooda/article/details/38823745](http://blog.csdn.net/vsooda/article/details/38823745)
    ![](http://ww2.sinaimg.cn/large/0060jr72gw1erqyo8vv3bj30ky0k1tfn.jpg)
    
### 4：在视觉的森林里漫游
看看艺术家们是如何将图像处理的融合技术运用于他们的领域
    诗歌辞藻文艺美妙，然后有时却晦涩难懂，而相比之下图片就简单易懂很多。摄影师Ted Chin自称是个不会用文字表达的人，因此他通过影像来传达自己的想法。欣赏Ted Chin的作品就像是在视觉的森林里漫游，在这个没有不可能的世界中，乍看下一切都那么的自然，然而当你细细琢磨却又发现处处充满着惊奇。
     
    
[http://news.bingodu.com/2015/05/02/nh/679292.html?cid=46&nid=53333&share=1&fuid=266960](http://news.bingodu.com/2015/05/02/nh/679292.html?cid=46&nid=53333&share=1&fuid=266960)
     ![](http://ww2.sinaimg.cn/large/0060jr72gw1erqz06mltxj30hs0hs77f.jpg)
![](http://ww2.sinaimg.cn/large/0060jr72gw1erqz07ji04j30hs0hsq5g.jpg)
![](http://ww1.sinaimg.cn/large/0060jr72gw1erqz088va3j30hs0hsaaq.jpg)

![](http://ww3.sinaimg.cn/large/0060jr72gw1erqz08hpzej30hs0hsjsg.jpg)
![](http://ww1.sinaimg.cn/large/0060jr72gw1erqz08v3wdj30hs0hs75r.jpg)
![](http://ww1.sinaimg.cn/large/0060jr72gw1erqz094wbgj30hs0hsjsj.jpg)
     

5：有网友说“算法这东西就跟女人一样，很搞，很作，但很有意思"
做图像视觉类的算法，如果能引入一些其他领域的知识，那可以让我们的算法工作立刻变得高大上并且有趣多了。
