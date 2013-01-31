---
layout: post
title: "Finger Gesture of Google Map App"
description: ""
category: 
tags: []
---
{% include JB/setup %}
##One Finger
Camera.x, Camera.y changes with touchPoint.x touchPoint.y

##Two Finger
1.两个 touchPoint 中点定义为相机的 target
2.同时识别
	touchpoint 间距变化 
	旋转角度

3.从单指转换到双指不执行俯仰角变换
4.刚开始即是双指,执行俯仰角变换
##Conclution
	if fingerCount ==1
	{
		middlePoint = (touchPoint0 + touchPoint1) /2
	}

	if (two fingers touch the screen at the same time)
	{
		Camera.rotate(0,(touchPoint0.deltaY + touchPoint1.deltaY),0);
		
	}