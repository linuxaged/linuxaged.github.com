---
layout: post
title: "Master Objective c Programming"
description: ""
category: 
tags: []
---
{% include JB/setup %}
#基本
类定义

	@interface MyClass : NSObject
	{
	NSString *name;
	}
	+ (void)aClassMethod;				 				// 类方法,用来构造实例
	- (void)anInstanceMethod;			 				// 实例方法, 对应c++中的成员方法
	@property (nonatomic, retain) NSString *myVar;		// property 属性不能直接获得,需要 self.myVar		
														// nonautomic 	  vs. 	atomic
														// 
														// readwrite 	  vs. 	readonly
														// 
														// retain 		  vs. 	copy 		vs. assign
														// 1. 假设你用malloc分配了一块内存，并且把它的地址赋值给了指针a，后来你希望指针b也共享这块内存，于是你又把a赋值给（assign）了b。此时a和b指向同一块内存，请问当a不再需要这块内存，能否直接释放它？答案是否定的，因为a并不知道b是否还在使用这块内存，如果a释放了，那么b在使用这块内存的时候会引起程序crash掉。 
														// 2. 了解到1中assign的问题，那么如何解决？最简单的一个方法就是使用引用计数（reference counting），还是上面的那个例子，我们给那块内存设一个引用计数，当内存被分配并且赋值给a时，引用计数是1。当把a赋值给b时引用计数增加到2。这时如果a不再使用这块内存，它只需要把引用计数减1，表明自己不再拥有这块内存。b不再使用这块内存时也把引用计数减1。当引用计数变为0的时候，代表该内存不再被任何指针所引用，系统可以把它直接释放掉。 
														// 3. 上面两点其实就是assign和retain的区别，assign就是直接赋值，从而可能引起1中的问题，当数据为int, float等原生类型时，可以使用assign。retain就如2中所述，使用了引用计数，retain引起引用计数加1, release引起引用计数减1，当引用计数为0时，dealloc函数被调用，内存被回收。 
														// 4. copy是在你不希望a和b共享一块内存时会使用到。a和b各自有自己的内存。 
														// 5. atomic和nonatomic用来决定编译器生成的getter和setter是否为原子操作。在多线程环境下，原子操作是必要的，否则有可能引起错误的结果。加了atomic，setter函数会变成下面这样： 
	@end

	[MyClass aClassMethod];								// 类方法
			
	MyClass *object = [[MyClass alloc] init]; 			//成员方法
	[object anInstanceMethod];

