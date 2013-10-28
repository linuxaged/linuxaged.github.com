---
layout: post
title: "Common Go Patterns"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Handle error everywhere
在所有可能抛出错误的地方进行错误处理, 这是 go 程序健壮的原因之一
	
	if err != null
#Zero Initialization
所有变量在声明的时候即被初始化为 0.