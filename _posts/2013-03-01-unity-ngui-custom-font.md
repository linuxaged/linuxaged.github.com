---
layout: post
title: "Unity NGUI custom font"
description: ""
category: 
tags: []
---
{% include JB/setup %}
#Reference
http://www.u3dchina.com/t-1394-1-1.html

#中文字体贴图制作
使用工具 GlyphDesigner
1.复制需要的文字到右侧"开 Included Glyphs" 工具栏里
2.在左侧字体选项中选择一个你喜欢的字体
3.其余设置:字体大小,阴影效果等等
4.导出.得到两个文件: xxx.png, xxx.fnt
#NGUI Atlas Prefab的制作
1.xxx.fnt 文件后缀改成 .txt 把两个文件倒入 Unity3D
2.NGUI -> Font Maker
3.Font Data 对应 xxx.txt ; Texture 对应 xxx.png ; 填写 Font Name; Atlas 选择 SciFi Atlas
4.Create