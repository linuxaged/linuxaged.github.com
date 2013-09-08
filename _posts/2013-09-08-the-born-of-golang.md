---
layout: post
title: "The born of Golang"
description: ""
category: 
tags: []
---
{% include JB/setup %}

2007年 Rob Pike, Ken Thompson, Robert Griesemer 因为对现有编程语言共同的不满决定开始设计一种全新的设计语言.

Go 作者们在介绍 Go 项目时指出:
	
	已经十几年没有出现新的系统语言了,但此时计算环境发生了巨大变化,出现了如下趋势:
	1)计算机的运行速度极快,而软件开发速度基本没变
	2)依赖管理是今天软件开发的一大部分,但传统的C语言的头文件是干净的依赖分析和快速编译的对立面
	3)反抗 Java, c++等语言笨重的类型系统的起义已在壮大,推动者人们使用python,javascript等动态类型系统语言.
	4)一些基本概念并未在主流语言中得到支持,例如垃圾回收,并行计算
	5)多核计算机的兴起带来了担忧和困扰