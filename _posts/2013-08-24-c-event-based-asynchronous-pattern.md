---
layout: post
title: "C# Event based Asynchronous Pattern"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#声明异步方法
为需要同步方法添加异步副本, 例如

	void Method_0() -> void Method_0_Async()
	void Method_1() -> void Method_1_Async()
	
如果你需要同步调用你的方法, 需要多一个参数的重载函数, 例如

	void Method_0_Async(Object userState)

这儿的 userState 会回传给所有的 EventHandler, 用来分辨函数的调用者
	