---
layout: post
title: "How Monotouch works"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Reference

[How Monotouch Works (Stackoverflow)](http://stackoverflow.com/questions/1453355/how-monotouch-works)

[How Mono works](http://stackoverflow.com/a/5188580/853569)

观察 Unity 导出的 Xcode 工程会发现,所有的秘密都隐藏在下面这几个文件中:

	libiPhone-lib.a
	Assembly-CSharp-firstpass.dll.s
	Assembly-CSharp.dll.s
	Assembly-UnityScript.dll.s
	Boo.Lang.dll.s
	Mono.Security.dll.s
	System.Core.dll.s
	System.dll.s
	UnityEngine.dll.s
	mscorlib.dll.s

