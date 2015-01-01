---
layout: post
title: "Compiler Design Part One"
description: ""
category: 
tags: []
---
{% include JB/setup %}
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


