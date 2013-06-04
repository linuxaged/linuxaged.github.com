---
layout: post
title: "c sharp design patterns"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Reference
[C# 设计模式](http://www.cnblogs.com/zhenyulu/category/6930.html)

#MVVM
EZGUI and EZData

EZAlignement components track their position and visibility changes and refresh bound ezgui elements when needed.
每个UI控件都会绑定一个 EZAlignement,用来更新UI的图像
而UI 事件的管理是通过 Context 类, Context 类在 Start 的时候初始化
Context
{
 bool onOff;
 string labelText;
 
 }
#Structural Patterns
##Decorator
提供了一种可以动态添加state, behavior的途径
##Proxy
##Bridge
#Composite and Flyweight patterns
#Adapter and Facade patterns