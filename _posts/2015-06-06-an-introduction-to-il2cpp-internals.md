---
layout: post
title: "An Introduction to IL2CPP Internals"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#What is IL2CPP

IL2CPP 由两部分组成：

* 一个 ahead-of-time（AOT） 编译器
* 一个用来支撑虚拟机的运行时库

#AOT 编译器

IL2CPP 的 AOT 编译器叫做 il2cpp.exe ，Windows 平台它在 `Editor\Data\il2cpp` 目录下，Mac 平台它在 `Contents/Frameworks/il2cpp/build` 目录下，il2cpp.exe 完全是由 C# 写的，所以它是一个 Managed 程序。在开发 IL2CPP 的时候，我们会同时用 .NET 和 Mono 编译器编译它。

il2cpp.exe 工具接收来自 Mono 编译好的 C# 汇编代码然后生成 C++ 代码输出到各平台的 C++ 编译器进一步编译成 native 的代码。

il2cpp.exe 的工作原理图：

![il2cpp toolchain](/assets/images/il2cpp-toolchain-smaller.png)

#运行时库

IL2CPP 的另外一部分就是支撑虚拟机的运行时库。我们用 C++ 实现了整个库（当然还有一些平台特殊的汇编），这个库叫做 libil2cpp , 他会被静态链接到最终的可执行程序。
IL2CPP 技术的一个关键优势就是这个简单的可移植的运行时库。