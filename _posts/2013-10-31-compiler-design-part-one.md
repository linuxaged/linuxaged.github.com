---
layout: post
title: "Compiler Design Part One"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Parser
传统手写 parser

	void stat() {		if ( «lookahead token is return» )
			returnstat(); 		else if ( «lookahead token is identifier» )
			assign();		else if ( «lookahead token is if» )
			ifstat(); 		else «parse error»	}

利用 DSL(例如 [ANTLR v4](https://github.com/antlr/grammars-v4)) 帮助我们构建 parser

识别关键字：

首先想想语言里面最最基础的元素有哪些？

保留字符窜：可以是单词、符号、字母数字组合等等

特殊字符：例如 ';'

用户自定义字符窜：


#AST

下面演示如何用 ANTLR 来构建 Abstract Syntax Trees

#Interpreter

memory space:

	globle 
	function space 
	struct or object instance
	
#[Runtime](http://cs.nyu.edu/~gottlieb/courses/2007-08-fall/compilers/lectures/lecture-13.html)
stack 分配。如何解决递归情况下的 stack 分配

`activation tree`

`Action Records` or `frame` 中包含：
	
	临时区 // 寄存器放不下时使用
	Data local
	status from caller: 返回地址；寄存器值
	access link
	返回值
	参数
	
	
a


	code |
	-----
	data |
	-----
	stack|
	-----
	heap

[llvm memory modle](http://llvm.org/docs/tutorial/LangImpl7.html#memory-in-llvm)