---
layout: post
title: "How CSharp GC Works"
description: ""
category: 
tags: []
---
{% include JB/setup %}

[MSDN: GC](http://msdn.microsoft.com/en-us/library/ms973837.aspx)

[Mono: Generational GC](http://www.mono-project.com/Generational_GC)

[Unity3D: Understanding Automatic Memory Management](http://docs.unity3d.com/Documentation/Manual/UnderstandingAutomaticMemoryManagement.html)

[移动端实用脚本优化](http://docs.unity3d.com/Documentation/Manual/iphone-PracticalScriptingOptimizations.html)

值得注意的是 Mono 的 GC 和微软的不一样， Unity 的 GC 又和 Mono 的不一样。

	MyClass mc = new MyClass();
	...
	mc = null;
	Destroy(mc);