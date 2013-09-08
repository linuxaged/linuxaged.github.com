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
	
###2.区分接口和抽象类的应用场合

抽象类解决的是 "Is a" 的问题, 接口解决的是 "Can do".

以 IDisposable 为例

public interface IDisposable
{
	void Dispose();
}
socket 和 DbConnection 都需要被释放, 但他们是不同的类型, 所以可以用接口统一他们的行为.
我们不能让完全不同的类继承同一个抽象类.	

###3.使用私有构造函数强化单例

	public sealed class Singleton
	{
		static Singleton instance;
		static readonly object padlock = new object();
		// 私有构造函数, 防止构造多个单例
		Singleton()
		{
		}
		public static Singleton Instance()
		{
			get
			{
				if (instance == null)
				{
					// 线程安全
					lock(padlock)
					{
						if (instance == null)
						{
							instance = new Singleton();
						}
					}
				}
				return instance;
			}
		}
	}
	
###4.[区分静态类和单例](http://msdn.microsoft.com/en-us/library/79b3xss3\(v=vs.90\).aspx)
静态类无法继承,无法作为参数和返回值传递.

静态类破坏了面向对象三大特性中的两项: 继承和多态