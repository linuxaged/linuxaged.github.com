---
layout: post
title: "Unity Instantiate Problems"
description: ""
category: 
tags: []
---
{% include JB/setup %}

	rankRows[i] = Instantiate(PrefabRankRow) as GameObject;
	
一个物体在 OSX Editor 里面默认隐藏 可以 Instantiate, 但到 iOS 上面就不行了, 必须取消隐藏.

记载在这里,防止下次再掉进去.