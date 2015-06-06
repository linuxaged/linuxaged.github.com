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

在 Unity 的安装目录

    Editor\Data\PlaybackEngines\webglsupport\BuildTools\Libraries\libil2cpp\include (Windows)
    Contents/Frameworks/il2cpp/libil2cpp (Mac)

下包含了运行时库的头文件，你可以通过这些头文件窥探 libil2cpp 代码是如何组织的。比如 il2cpp.exe 生成的 C++ 代码和 libil2cpp 之间的接口在 codegen/il2cpp-codegen.h 头文件中。

运行时的一个重要部分就是 GC。在 Unity 5 中用的是 [libgc](https://github.com/ivmai/bdwgc/), 一个 Boehm-Demers-Weiser  算法的垃圾回收器。实际上 libil2cpp 被设计成可以使用任何垃圾回收器。例如我们正在研究如何继承微软 CoreCLR 中的垃圾回收器。关于垃圾回收我们会在后面专门写一篇文章讨论。

#il2cpp 如何执行？

我们来举个例子。用 Unity 5 建一个空的工程和场景，然后绑定一个 MonoBehaviour 脚本到 Main Camera

    using UnityEngine;

    public class HelloWorld : MonoBehaviour {
      void Start () {
        Debug.Log("Hello, IL2CPP!");
      }
    }

当我们编译到 WebGL 平台的时候，可以用 Process Explorer 来观察 Unity 用哪些命令来执行 il2cpp:

    "C:\Program Files\Unity\Editor\Data\MonoBleedingEdge\bin\mono.exe" "C:\Program Files\Unity\Editor\Data\il2cpp/il2cpp.exe" --copy-level=None --enable-generic-sharing --enable-unity-event-support --output-format=Compact --extra-types.file="C:\Program Files\Unity\Editor\Data\il2cpp\il2cpp_default_extra_types.txt" "C:\Users\Josh Peterson\Documents\IL2CPP Blog Example\Temp\StagingArea\Data\Managed\Assembly-CSharp.dll" "C:\Users\Josh Peterson\Documents\IL2CPP Blog Example\Temp\StagingArea\Data\Managed\UnityEngine.UI.dll" "C:\Users\Josh Peterson\Documents\IL2CPP Blog Example\Temp\StagingArea\Data\il2cppOutput"

这个命令比较长，分解一下：

    "C:\Program Files\Unity\Editor\Data\MonoBleedingEdge\bin\mono.exe"

    "C:\Program Files\Unity\Editor\Data\il2cpp/il2cpp.exe"