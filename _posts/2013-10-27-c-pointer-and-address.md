---
layout: post
title: "C Pointer and Address"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Reference
 
`Programming Language Pragmatics` chapter 7.7

`C++ 反汇编和逆向分析技术揭秘` chapter 8

##区别
指针是一个**高级概念**，是对于对象的引用；

地址是一个**低级概念**，是内存单元的位置。

##联系
指针通常通过地址实现，但不总是这样。

在具有分段存储体系结构的机器上，指针可以由一个
段标识和一个段内偏移量组成.

在那些试图捕捉所有悬空引用的语言中, 指针可能包含一个地址和一个 access key

##作为运算符的时候
	int n;
	int *a;
	int b[10];
	a = b; // 指向 b 的第一个元素
	n = b[1];
	n = *(b+1);

`[ ]` 运算符其实是指针运算的语法糖. 所以 `n = b[1]`, 是对于 `n = *(b+1)` 的包装

##声明的时候

	int *a; // 分配 一个指针类型的内存
	int a[10]; // 分配 10 个 int 类型的内存
	
##下标寻址和指针寻址
	#include <stdio.h>
	int main()
	{
		char * pChar = NULL;
		char array[] = "Hello";
		pChar = array;
		printf("%c\n", *pChar);
		printf("%c\n", array[0]);
	}
	
	
`gcc -S -O3 -fno-asynchronous-unwind-tables test.c`

对应的汇编

		.section	__TEXT,__text,regular,pure_instructions
		.globl	_main
		.align	4, 0x90
	_main:                                  ## @main
	## BB#0:
		pushq	%rbp
		movq	%rsp, %rbp
		pushq	%rbx
		pushq	%rax
		leaq	L_.str(%rip), %rbx
		movq	%rbx, %rdi
		movl	$72, %esi
		xorb	%al, %al
		callq	_printf
		movq	%rbx, %rdi
		movl	$72, %esi
		xorb	%al, %al
		callq	_printf
		xorl	%eax, %eax
		addq	$8, %rsp
		popq	%rbx
		popq	%rbp
		ret
	
		.section	__TEXT,__cstring,cstring_literals
	L_.str:                                 ## @.str
		.asciz	 "%c\n"
	
	
	.subsections_via_symbols

太血腥了, 在 O3 优化后, 他居然直接用 H 的 ascii 码值代替了寻址.

此处不能优化

		.section	__TEXT,__text,regular,pure_instructions
		.globl	_main
		.align	4, 0x90
	_main:                                  ## @main
	## BB#0:
		pushq	%rbp
		movq	%rsp, %rbp
		subq	$32, %rsp
		movq	$0, -8(%rbp)
		movl	L_main.array(%rip), %eax
		movl	%eax, -14(%rbp)
		movw	L_main.array+4(%rip), %cx
		movw	%cx, -10(%rbp)
		leaq	L_.str(%rip), %rdi
		leaq	-14(%rbp), %rdx
		movq	%rdx, -8(%rbp)
		movq	-8(%rbp), %rdx
		movsbl	(%rdx), %esi
		movb	$0, %al
		callq	_printf
		leaq	L_.str(%rip), %rdi
		movsbl	-14(%rbp), %esi
		movl	%eax, -20(%rbp)         ## 4-byte Spill
		movb	$0, %al
		callq	_printf
		movl	$0, %esi
		movl	%eax, -24(%rbp)         ## 4-byte Spill
		movl	%esi, %eax
		addq	$32, %rsp
		popq	%rbp
		ret
	
		.section	__TEXT,__cstring,cstring_literals
	L_main.array:                           ## @main.array
		.asciz	 "Hello"
	
	L_.str:                                 ## @.str
		.asciz	 "%c\n"
	
	
	.subsections_via_symbols

