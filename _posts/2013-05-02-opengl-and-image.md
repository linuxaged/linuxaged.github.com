---
layout: post
title: "OpenGL and Image"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#References
[OpenGL像素缓冲区对象](http://blog.csdn.net/dreamcs/article/details/7708018)

#定义
与 bitmap 不同,image 中的每个像素可以包含多个信息,R.G.B.A
OpenGL 提供了三个操作图像的函数:

	glReadPixals() // 从 frame buffer 读取像素数据到内存
	glCopyPixals() // 像素数据在 fb 之间拷贝
	glDrawPixals() // 将内存里的像素数组写入到 fb, 有 glRasterPos()指定位置
	
#Creating PBO
1.Generate a new buffer object with `glGenBuffersARB()`.

2.Bind the buffer object with `glBindBufferARB()`.

3.Copy pixel data to the buffer object with `glBufferDataARB()`.

#典型应用

文理更新和异步读取