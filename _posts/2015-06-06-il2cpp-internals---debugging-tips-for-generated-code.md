---
layout: post
title: "IL2CPP Internals   Debugging Tips For Generated Code"
description: ""
category:
tags: []
---
{% include JB/setup %}

这是 IL2CPP Internals 系列的第三篇。这篇我们会讲关于如何调试生成的 C++ 代码。比如如何打断点，查看变量的值，定位异常。

#配置

就用前面的例子工程，不过这次我们把 Build Target 设成 iOS 然后选择使用 IL2CPP backend.同样把 Development Player 选项选上，方便 il2cpp.exe 按照 IL 代码的名称生成 C++

编译完了用 Xcode 打开工程。

#设置断点

在运行工程之前我们在 HelloWorld 的 Start 方法处打个断点。参考上篇文章翻译后的 Start 方法应该叫做 HelloWorld_Start_xx，我们使用快捷键 Cmd+Shift+O 开始搜索这个方法，然后设置断点。

#观察 string 类型

有两种方法可以观察 IL2CPP 的 string 类型。一种直接看内存数据，一种使用 il2cpp 提供的一个转化工具把 string 转化成 std::string, 然后就可以直接在 Xcode 里看到。我们来看一下值为 "Hello, IL2CPP" 的字符串: _stringLiteral1

我们在 Ctags 里面搜索然后找到它，发现它是一个 IL2CppString_14 类型：

    struct Il2CppString_14
    {
      Il2CppDataSegmentString header;
      int32_t length;
      uint16_t chars[15];
    };

其中 header 是所有托管类型都包含的标准头部 IL2CppObject （这里是 typedef 后的 Il2CppDataSegmentString）, 然后是 4 字节的 length 最后是双字节字符串数组，这表示的是 _stringLiteral1 所以用定长数组，如果是运行时创建的 string 就要用堆上的数组。里面的字符是 UTF-16 格式的编码。

如果我们把 _stringLiteral1 加到 Xcode 的 watch window 里，就可以右击选择 View Memory 来查看字符串内容。


