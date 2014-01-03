---
layout: post
title: "OpenGL ES Shader"
description: ""
category: 
tags: []
---
{% include JB/setup %}

很久以来没能理解 shader 的原因就是不明白什么是 modelViewProjectionMatrix 变换.

首先这个变换由两个矩阵级联而成 : modelview 矩阵 和 projection 矩阵; modelview 作用于整个场景, projection 作用于场景内模型显示位置的坐标系
