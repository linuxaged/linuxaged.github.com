---
layout: post
title: "Cpp Multi Thread Programming"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Reference
[c++11 thread support library](http://en.cppreference.com/w/cpp/thread)
#c++11
std::share_ptr
#线程管理
##Launching
	void do_some_work();	std::thread my_thread(do_some_work);
##Waiting
	my_thread.join();
##Running background
	my_thread.detach()
##Ownership
	**std::thread**

	join
	detach
	swap
	
#线程间数据共享

	std::lock_guard
	
#内存模型和原子类型操作

#设计基于锁的并行数据结构