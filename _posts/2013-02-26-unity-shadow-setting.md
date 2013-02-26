---
layout: post
title: "unity shadow setting"
description: ""
category: 
tags: []
---
{% include JB/setup %}
#Refence
http://docs.unity3d.com/Documentation/Manual/Shadows.html

http://www.gameres.com/thread_182344_1_1.html

利用projector 制作实时投影
http://www.youtube.com/watch?annotation_id=annotation_636868&feature=iv&src_vid=WezpIHO_crw&v=994_6GZo3hs
目前最好选择非实时的阴影解决方案
1.设置模型 Inspector -> Model -> select -> Model -> Generate Lightmap UVs
2.设置 Light 的 Shadow Type: Hard Shadow / Soft Shadow
3.要投影的物体设置为 static
4.Window -> Lightmapping, 对各选项进行配置
5.关闭相机的 shadow