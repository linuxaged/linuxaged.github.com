---
layout: post
title: "Android and iOS Devices Screen Resoluton List"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Android

需要适配的分辨率:
			
			if (Screen.width == 800 && Screen.height == 480)
			{
				UI.localScale = new Vector3(1.12f, 1.0f, 1.0f);
			}
			if (Screen.width == 854 && Screen.height == 480)
			{
				UI.localScale = new Vector3(1.2f, 1.0f, 1.0f);
			}
			if (Screen.width == 1024 && Screen.height == 600)
			{
				UI.localScale = new Vector3(1.15f, 1.0f, 1.0f);
			}
			if (Screen.width == 1280 && Screen.height == 800)
			{
				UI.localScale = new Vector3(1.08f, 1.0f, 1.0f);
			}
			if (Screen.width == 1280 && Screen.height == 720)
			{
				UI.localScale = new Vector3(1.19f, 1.0f, 1.0f);
			}
			
			
#iOS
##iPhone
###4, 4S 
 	960 * 640
 	
###5
 	1136 * 640
 	
##iPad
###1,2
	1024 * 768
###3,4
	2048 * 1536