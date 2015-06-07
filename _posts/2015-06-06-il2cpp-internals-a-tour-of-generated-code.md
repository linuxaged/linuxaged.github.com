---
layout: post
title: "IL2CPP Internals: A Tour of Generated Code"
description: ""
category:
tags: []
---
{% include JB/setup %}

这是 il2cpp internals 系列的第二篇，着重研究一下 il2cpp.exe 生成的代码。我们将看到如何用 native 代码表示托管类型；如何用运行时检测来支撑 .NET 虚拟机；循环是如何表示的。

不同版本的 Unity 生成的代码可能存在巨大差异，但是概念是一样的。

#例子工程

我们用最新的 5.0.1p1 版本。新建一个空的工程，添加一个脚本：

    using UnityEngine;

    public class HelloWorld : MonoBehaviour {
      private class Important {
        public static int ClassIdentifier = 42;
        public int InstanceIdentifier;
      }

      void Start () {
        Debug.Log("Hello, IL2CPP!");

        Debug.LogFormat("Static field: {0}", Important.ClassIdentifier);

        var importantData = new [] {
          new Important { InstanceIdentifier = 0 },
          new Important { InstanceIdentifier = 1 } };

        Debug.LogFormat("First value: {0}", importantData[0].InstanceIdentifier);
        Debug.LogFormat("Second value: {0}", importantData[1].InstanceIdentifier);
        try {
          throw new InvalidOperationException("Don't panic");
        }
        catch (InvalidOperationException e) {
          Debug.Log(e.Message);
        }

        for (var i = 0; i < 3; ++i) {
          Debug.LogFormat("Loop iteration: {0}", i);
        }
      }
    }

同样，我们选择 WebGL 为编译目标，然后在 Build Settings 里面把 Development Player 选项勾上，以便生成可读性更强的代码。另外再把 Enable Exception 选项设为 Full

#总览

WebGL build 结束后生成的 C++ 代码在 Temp\StagingArea\Data\il2cppOutput 目录下。Editor 关掉这个目录就会被删除。

即使是例子这么小的工程 il2cpp 都生成了一大堆的文件。有 4625 个头文件, 89 个 C++ 源文件。
为了方便我们的分析，可以利用 Exuberant CTags 这个工具生成一些标签，然后我们可以在标签之间来回跳转了。

#托管代码如何对应到生成的 C++ 代码

il2cpp.exe 会为每个托管类型生成两份 C++ 头文件，一份包含类型定义，一份包含所有这个类型相关的方法。

以 UnityEngine.Vector3 为例，生成的头文件叫做 `UnityEngine_UnityEngine_Vector3.h`, 这是以汇编代码命名的，汇编代码名称(UnityEngine.dll) + 汇编的名空间(UnityEngine) + 类型名称 (Vector3)

生成的代码如下：

    // UnityEngine.Vector3
    struct Vector3_t78
    {
        // System.Single UnityEngine.Vector3::x
        float ___x_1;
        // System.Single UnityEngine.Vector3::y
        float ___y_2;
        // System.Single UnityEngine.Vector3::z
        float ___z_3;
    };

另一个 `UnityEngine_UnityEngine_Vector3MethodDeclarations.h` 头文件包含了所有和 Vector3 方法的声明，比如 Vector3 重载了 Object.ToString() 方法：

    // System.String UnityEngine.Vector3::ToString()
    extern "C" String_t* Vector3_ToString_m2315 (Vector3_t78 * __this, MethodInfo* method)

注意一下这里的注释包含了表示的具体托管方法，我们可以利用这个来搜索到相关的托管代码最终被翻译成的 C++ 代码。

有几个地方值得注意：

    * C++ 里面的所谓成员函数其实是包含了一个 this 指针参数的普通函数。il2cpp 翻译方法的时候默认第一个参数都是 this, 只不过翻译静态函数的时候这个 this 指针会被设置为 NULL，这极大的简化了翻译工作同时方便翻译完的方法相互调用。

    * 每个方法都会有个 MethodInfo* 的参数，它包含了调用 virtual 方法时候必要的信息。Mono 的做法是用平台特殊的 trampolines 传递它。il2cpp 为了更好的可移植性没有使用这个。

    * 所有的方法都被声明成 extern "C" , 因而必要的时候我们可以欺骗 C++ 编译器我们的方法有着相同的类型。

    * 类型名称都有一个 "_t" 后缀。方法名称都有一个 "_m" 后缀。为了防止命名冲突我们会为每个名字加入一个特殊的数字。只要原来的脚本变动了重新编译的时候这个数字也变了。

前两点说明了每个方法至少有两个参数，this 指针和 MethodInfo 指针。相比原来的方法这两个额外的参数会造成性能损失吗？理论会，但实际 profile 的时候，两者之间的性能差距可以忽略。

我们通过 Ctags 跳转到 ToString 方法。在 Bulk_UnityEngine_0.cpp 文件里。你会发现翻译之后的定义和原来 c# 的定义很不一样，但如果你用 ILSpy 这样的工具反汇编 Vector3::ToString() 方法的话，会发现生成的 C++ 代码和 IL 代码差不多

Bulk_UnityEngine_0.cpp 这个文件是在太大了，有 两千多行，il2cpp 为什么不把每个方法放在单独一个文件里呢？因为我们发现 C++ 编译器编译大量的源文件的时候非常非常慢。

#总结

