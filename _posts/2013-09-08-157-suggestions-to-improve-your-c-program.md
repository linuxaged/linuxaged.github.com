---
layout: post
title: "157 Suggestions to Improve Your C# Program"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#架构篇
###1.用多态代替条件语句

	// 定义抽象类
	abstract class Commander
	{
		public override void Execute()
		{
		
		}
	}
	// 重载
	class StartCommander: Commander
	{
		public override void Execute()
		{
			// 开始
		}
	}												
	class StopCommander: Commander
	{
		public override void Execute()
		{
			// 停止
		}
	}