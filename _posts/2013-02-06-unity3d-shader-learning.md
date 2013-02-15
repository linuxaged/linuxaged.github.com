---
layout: post
title: "unity3d shader learning"
description: ""
category: 
tags: []
---
{% include JB/setup %}
##效果
###combine texture
	Pass
	{
		Color [_Color]
		SetTexture[_MainTexture] {Combine texture * primary}
	}