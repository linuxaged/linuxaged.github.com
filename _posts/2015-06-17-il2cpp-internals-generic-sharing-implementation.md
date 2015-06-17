---
layout: post
title: "IL2CPP Internals: Generic Sharing Implementation"
description: ""
category: 
tags: [il2cpp]
---
{% include JB/setup %}


什么是 generic sharing?

你给 List<T> 实现一个 Add 方法，当你分别调用 List<string> 、 List<object> 、 List<DateTime> 类型的 Add 方法的时候，他们的 Add 方法是否共用？

模板的强大在于这些 Add 方法是共享的。但当我们把 C# 代码翻译成 Mono 或者 C++ 的时候他们的实现也能共享吗？

大多数时候可以。能不能共享取决于 T 类型的大小，如果 T 是个引用类型（像 string 或者 object），那它的大小就是一个指针的大小；如果 T 是值类型（比如 int 或者 DataTime）情况就复杂些，它的大小可能不一致。

#总结

Generic Sharing 通过共享方法的实现显著减小了生成的 C++ 代码的体积。我们一直在努力减少代码体积，看还能不能进一步地共享方法的实现。

在下一篇我们会探讨 p/invoke 包装是如何生成的，类型是如何从托管编导