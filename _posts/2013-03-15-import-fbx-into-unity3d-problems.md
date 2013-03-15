---
layout: post
title: "Import FBX into Unity3d problems"
description: ""
category: 
tags: []
---
{% include JB/setup %}
#步骤
1.安装 fbx 导出插件
2.选中需要导出的物体,File -> Export, 使用默认导出选项即可
3.拷贝导出文件所在文件夹到 Unity 工程目录下
4,把 fbx 文件拖入 scene 中
#导出骨骼动画

#光照贴图
Unity 内置了光照贴图烘培引擎 lightmapping,

如果你想使用 fbx 文件中的法线, 就需要关闭 import settting 中的 automatic normal generation

Unity3D 把 fbx 模型拆开,在分界线上分出新的顶点,这回导致顶点数的增加