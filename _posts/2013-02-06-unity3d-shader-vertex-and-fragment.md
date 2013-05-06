---
layout: post
title: "unity3d shader vertex and fragment"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Reference
[Shading theory](http://www.amaanakram.com/tutorials/shading_theory/)
[Providing vertex data to vertex programs](http://docs.unity3d.com/Documentation/Components/SL-VertexProgramInputs.html)
#基础
##Vertex Shader

可编程管道允许你通过 shader 来控制 顶点和像素的变换.
vertex shader 能干什么?

    Vertex transformations.
    Normal transformations.
    Normal normalization (i.e., turn it into a unit vector). 
    Handling of per-vertex lighting.
    Handling of texture coordinates.
不能干什么?

    View volume clipping. 
    Homogeneous division. 
    Viewport mapping.
    Backface culling.
    Polygon mode.
    Polygon offset.

在 unity 中每个物体都有一个 mesh renderer, 他负责调用 opengl 函数 把 mesh 相关的参数 (顶点坐标,法线,UV)传入流水线,包括:

    Vertex: 顶点：顶点的位置
    Normal: 法线：顶点的法线
    Tangent: 切线：顶点的切线
    Texcoord: 主要的UV坐标
    Texcoord1: 次要的UV坐标
    Color: 颜色：每个顶点颜色
    
你可以自定义一个结构来传递参数:

	struct vertexInput {
         float4 vertex : POSITION;
         float4 tangent : TANGENT;
         float3 normal : NORMAL;
         float4 texcoord : TEXCOORD0;
         float4 texcoord1 : TEXCOORD1;
         fixed4 color : COLOR;
      };
      
也可以使用内置的,只需要包含头文件 `#include "UnityCG.cginc"`

	       struct appdata_base {
              float4 vertex : POSITION;
              float3 normal : NORMAL;
              float4 texcoord : TEXCOORD0;
           };
           struct appdata_tan {
              float4 vertex : POSITION;
              float4 tangent : TANGENT;
              float3 normal : NORMAL;
              float4 texcoord : TEXCOORD0;
           };
           struct appdata_full {
              float4 vertex : POSITION;
              float4 tangent : TANGENT;
              float3 normal : NORMAL;
              float4 texcoord : TEXCOORD0;
              float4 texcoord1 : TEXCOORD1;
              fixed4 color : COLOR;
              // and additional texture coordinates only on XBOX360)
           };
           
##Fragment Shader

	View volume clipping. 
    Homogeneous division. 
    Viewport mapping.
    Backface culling.
    Polygon mode.
    Polygon offset.
 a
 
    Blending.
    Stencil test.
    Depth test.
    Scissor test.
    Stippling operations.
    Raster operations performed as a pixel is being written to the framebuffer.