---
layout: post
title: 一些并发编程的学习
description: ""
category: 
tag: 
- java
- concurrent

---
* content
{:toc}

作者@[NaturalL](https://github.com/NaturalL) [http://www.jianshu.com/p/70cd64dca48e](http://www.jianshu.com/p/70cd64dca48e)

对于并发编程，除了操作系统课上学的那点知识，我的实践很少。

本科时的实践，一个是用在聊天小程序上，一个线程收，一个线程发；还有是写界面程序时，GUI单独一个线程。仅此而已。

后来接触node.js 和redis, 了解了一些事件驱动的单线程并发模型。对于并发编程，比如如何用好多线程、锁、同步之类的，也挺好奇的，哈，我觉得这是一个提高自己的好方向。

第一本看的书是《Seven Concurrency Models in Seven Weeks》英文版PDF，即《七周七并发模型》, 互动出版网上也有中文翻译版卖。（嘿嘿，在互动出版网上买了我的第一本计算机书，《算法导论》）

这本书还是蛮不错的，起码给了我一个基本认识，还接触了一点函数式编程语言Clojure，认识了一点go、erlang的特性。

具体内容就不说了，感兴趣可以看看。这里列一下章节目录：

1. Introduction
2. Threads and Locks
3. Functional Programming
4. The Clojure Way—Separating Identity from State
5. Actors
6. Communicating Sequential Processes
7. Data Parallelism
8. The Lambda Architecture

每章分为3Day, 一周。

第二本书是实习时开始看的，只看了一半，还在看，叫《Java.Concurrency.in.Practice》。

据说这书经典，但是我看文字太多了，图比较少，似乎不太直观。此书谈到的问题还是很实用的，包含Java的内存模型，什么线程安全、内存可见性、同步方式、线程池、阻塞中断等等，还有 java.util.concurrent包里很多类的使用方法等等。

> 推荐:
> 并发编程网 http://ifeve.com/   
> InfoQ: http://www.infoq.com/cn

很多知识知道是知道一点，但是没有去用，总徘徊在门外。只有心里有兴趣，有很大疑惑，或者需要的时候才会去看。

有相关项目，一边做一边学，没事看看java.util.concurrent里一些类的实现方式，设计思路，也蛮不错的。
