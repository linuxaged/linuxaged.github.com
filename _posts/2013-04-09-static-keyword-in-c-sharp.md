---
layout: post
title: "Static Keyword in C Sharp"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#References
[link 1](http://msdn.microsoft.com/en-au/library/79b3xss3\(v=vs.80\).aspx)

Static classes and class members are used to create data and functions that can be accessed without creating an instance of the class. Static class members can be used to separate data and behavior that is independent of any object identity: the data and functions do not change regardless of what happens to the object. Static classes can be used when there is no data or behavior in the class that depends on object identity.

Static 关键字给我的感觉就是,形式上把数据和操作放到一个 class 里, 访问他们并不需要通过面向对象的方式(创建实例,通过实例来访问)