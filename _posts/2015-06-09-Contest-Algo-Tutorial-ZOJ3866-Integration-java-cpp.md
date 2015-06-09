---
layout: post
title: "投稿：[微积分应用]求体积、面积并写出相应的(竞赛)程序"
date: 2015-06-09 20:54:00
tag: 
- ACM-ICPC
- semprathlon
categories: acm
---

* content
{:toc}

[博文地址](http://blog.semprathlon.net/archives/750)   
[原题地址](http://acm.zju.edu.cn/onlinejudge/showProblem.do?problemCode=3866)   
比赛时卡在这种题上，很是要命   

> Edward the confectioner is making a new batch of chocolate covered candy. Each candy center is shaped as a cylinder with radius r mm and height h mm.
> 
> The candy center needs to be covered with a uniform coat of chocolate. The uniform coat of chocolate is d mm thick.
> 
> You are asked to calcualte the volume and the surface of the chocolate covered candy.
> 
> Input
> 
> There are multiple test cases. The first line of input contains an integer T(1≤ T≤ 1000) indicating the number of test cases. For each test case:
> 
> There are three integers r, h, d in one line. (1≤ r, h, d ≤ 100)
> 
> Output
> 
> For each case, print the volume and surface area of the candy in one line. The relative error should be less than 10^-8.
> 
> Sample Input
> 
> 2   
> 1 1 1   
> 1 3 5   
> 
> Sample Output
> 
> 32.907950527415 51.155135338077   
> 1141.046818749128 532.235830206285   
> 
> Author: ZHOU, Yuchen Source: The 15th Zhejiang University Programming Contest   

题意是说，在底面半径r mm，高h mm的圆柱体糖块外裹上一层厚度均匀的壳，求它的体积和表面积。   
设外壳厚度为a mm(只是为了避免与微分符号混淆，原题中用d表示)，   

# 求体积   
* (1)糖果顶上和底下的各一个侧面圆滑的类台体   
$$  z^2+{(y-r)}^2=a^2(y\ge r),y=\sqrt {a^2-z^2} +r.$$   
$$  
\begin{array}
\newline
V_{台体}=\int_0^a{\pi (\sqrt {a^2-z^2} +r)^2 {\rm d}z}
\newline\left( 或V_{台体}=\int_0^a {\rm d}z\int_0^{2\pi}{\rm d}\theta \int_0^{r+a\sqrt{1-\frac{z^2}{a^2}}}r{\rm d}r \right)
\newline=\int_0^a \pi \left(r+a\sqrt{1-\frac{z^2}{a^2}}\right)^2 {\rm d}z
\newline=\int_0^a \left( \pi r^2+\pi a^2\left(1-\frac{z^2}{a^2}\right)+2\pi ar\sqrt{1-\frac{z^2}{a^2}} \right) {\rm d}z
\newline=\int_0^a \left( \pi r^2+\pi a^2 \right) {\rm d}z-\int_0^a \pi z^2{\rm d}z + \int_0^a 2\pi r\sqrt{a^2-z^2}{\rm d}z
\newline=\pi a^3+\pi ar^2-\frac{\pi a^3}{3}+\frac{\pi^2 a^2r}{2}\newline
\end{array}
$$   

* (2)糖果中部的圆柱体   
$$  V_{圆柱}=Sh=\pi r^2h=\pi(r+a)^2h$$

* (3)总体积   
$$  V=2V_{台体}+V_{圆柱}=\frac{4\pi a^3}{3}+2\pi ar^2+\pi^2a^2r+\pi(r+a)^2h $$

# 求表面积   
* (1)求上下各一个环状曲面的面积   
这个曲面（轮胎面）的参数方程为
$$ 
\begin{cases}  
x = (r+a {\rm cos}\varphi) {\rm cos}\theta \newline  
y = (r+a {\rm cos}\varphi) {\rm sin}\theta \newline  
z = a {\rm sin}\varphi  
\end{cases}  
$$  
用向量值函数表示为：$$ \vec{f}(\theta,\varphi)=x(\theta,\varphi)\vec{i}+y(\theta,\varphi)\vec{j}+z(\theta,\varphi)\vec{k}$$
该曲面上某一点(x,y,z)处的基本法向量
$$ 
\vec{n}=(-\frac{\partial z}{\partial x},-\frac{\partial z}{\partial y},1)=\frac{\partial \vec{f}}{\partial \theta}\times\frac{\partial \vec{f}}{\partial \varphi}
$$  
积分区域$$  \Sigma={( \theta,\varphi ) | 0\le \theta \le 2\pi,0\le \varphi \le\frac{\pi}{2} } $$ 
$$  
\begin{array}
\newline
S_{环}=\iint_\Sigma {\rm d}S=\iint_\Sigma |\vec{n}| {\rm d}x{\rm d}y\newline
=\iint_\Sigma \sqrt{\left(-\frac{\partial z}{\partial x}\right)^2+\left(-\frac{\partial z}{\partial y}\right)^2+1}{\rm d}x{\rm d}y\newline
=\iint_\Sigma \left|\frac{\partial \vec{f}}{\partial \theta}\times\frac{\partial \vec{f}}{\partial \varphi}\right|  {\rm d}x{\rm d}y\newline
=\int_0^\frac{\pi}{2} {\rm d}\varphi \int_0^{2\pi} a(r+a {\rm cos}\varphi) {\rm d}\theta\newline
=\int_0^\frac{\pi}{2} 2\pi a(r+a {\rm cos}\varphi){\rm d}\varphi\newline
=2\pi a\left.(r+a {\rm cos}\varphi)\right|_0^\frac{\pi}{2}=\pi^2 ar+2\pi a^2\newline
\end{array}
$$  

* (2)求侧面积   
$$
S_{侧}=2 \pi(r+a)h
$$

* (3)求底面积   
$$
S_{底}=\pi r^2
$$

* (4)总表面积   
$$
S=S_{环}+S_{侧}+2S_{底}=\pi^2 ar+2\pi a^2+2 \pi(r+a)h+2 \pi r^2
$$
