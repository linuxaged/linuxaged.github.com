---
layout: post
title: "Render Vector on GPU"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Reference
[GPU Gems 3 Chapter25](http://http.developer.nvidia.com/GPUGems3/gpugems3_ch25.html)

[Rational Curves](http://www.cs.mtu.edu/~shene/COURSES/cs3621/NOTES/curves/rational.html)

[参数曲线和曲面](http://202.118.167.67/eol/data/res/jsjtxx/Chapter3/CG_Txt_3_015.htm)

[Bernstein Polynomial](http://mathworld.wolfram.com/BernsteinPolynomial.html)

#Basics
表示平面曲线的方程主要有

1. 显函数方程：y=f(x)
2. 隐函数方程(也叫一般方程)：F(x,y)=0
3. 参数方程: x=x(t), y=y(t)
4. 极坐标方程:


#Parametric Curves

C(t) = **t** · **C**


The vector **t** contains power basis functions and **C** is the coefficient matrix that determines the shape of the curve. The rational curve C(t) has components [x(t) y(t) w(t)]. In the special case where w(t) = 1, we refer to C(t) as an integral curve. Commonly, the parameter t is restricted to the interval [0,1] and we think of C(t) as defining a curve segment.

##Implicit Curves


**TrueType curves** are quadratic B-splines. Each spline is equivalent to a series of quadratic Bézier curves. And each of these is defined by 3 outline control points, rendered by a parametric, quadratic equation hard-coded into the rasterizer.
If the 3 points of each Bézier curve are (Ax, Ay), (Bx, By) and (Cx, Cy), then

`px = (1-t)2.Ax + 2t(1-t).Bx + t2.Cx`

`py = (1-t)2.Ay + 2t(1-t).By + t2.Cy`

Varying the parameter t from 0 to 1 produces all the points p on the curve defined by A, B and C.
#Quadratic Splines
B-spline -> Bézier