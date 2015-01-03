---
layout: post
title: "The Process Of Android Activity Launching"
description: ""
category: 
tags: []
---
{% include JB/setup %}

[Android OS - Process and Zygote](http://coltf.blogspot.com/p/android-os-processes-and-zygote.html)

[Android Application Launch](http://multi-core-dump.blogspot.com/2010/04/android-application-launch.html)

有两种基本的 Activity 类型：
	
	根 Activity ; 子 Activity
	

MainActivity 启动过程

	Launcher --(ActivityManagerService)--> MainActivity
	
这三个组件分别运行在不同的进程中，通过 Bind 通信来完成 MainActivity 的启动。

Android 操作系统在 boot 时会启动一个 `PackagedManagerService` 服务，负责管理应用程序的安装。在应用程序安装时， `PackagedManagerService` 会对应用的 AndroidMenifest.xml 进行解析，获得里面的组件信息。当你点击应用程序的图标时， 