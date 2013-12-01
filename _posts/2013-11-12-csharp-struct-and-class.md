---
layout: post
title: "CSharp struct and class"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Struct
值类型：栈上分配；不能为 null；不能引用同一个对象

隐式密封，不能派生

提供默认构造函数将成员设置成默认值；不能有析构函数（栈上分配嘛）

其实 基础类型 int, short , long 等在 C# 中都实现为结构
#装箱拆箱

将 struct 用作 应用对象会引起 boxing