---
layout: post
title: "CSharp array and foreach"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#概述
数组是从 system.Array 继承的对象

#分类
按照维度划分
#####一维
	int     [];
	MyClass [];
#####多维
**矩形数组**

	int     [,];
	MyClass [,];
**交错数组(jagged array)**

	int     [][];
	MyClass [][];

其实更准确的叫法应该是“联合一维数组”


	public class NewBehaviourScript : MonoBehaviour {
		// 换成 struct 类型试试
		class Player
		{
			public int id = 0;
			public string name = null;
		}
		
		void Start()
		{
			Player[] players = new Player[3];
			// 如果不初始化会出现什么情况
			for (int i=0; i<3; i++)
			{
				players[i] = new Player();
				players[i].id = i;
			}
			
			foreach (Player p in players)
			{
				p.id += 100;
			}
			foreach (Player p in players)
			{
				print(p.id);
			}
		}
	}
	
	输出:
	100
	101
	102
	

#Conclusion
当需要修改数组内元素时，foreach 的迭代对象只能是引用类型