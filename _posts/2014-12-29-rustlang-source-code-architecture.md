---
layout: post
title: "rustlang source code architecture"
description: ""
category: 
tags: [rust]
---
{% include JB/setup %}

liballoc
	
	arc.rs 		// Threadsafe reference-counted boxes (the `Arc<T>` type).
	boxed.rs 	// box 默认分配器
	heap.rs 	// 系统级的内存分配，调用 jemalloc
	rc.rs 		// Rc<T> 实现，类似 C++ share_ptr<T>
	
	
libarena

libbacktrace

	回溯，追踪；用于调试

libcollections

	btree/
	bench.rs
	binary_heap.rs
	bit.rs
	dlist.rs
	enum_sat.rs
	ring_buf.rs
	slice.rs
	str.rs
	string.rs
	vec.rs
	vec_map.rs

libcore
	
	fmt/
	hash/
	num/
	str/
	any.rs
	array.rs
	atomic.rs
	borrow.rs
	cell.rs
	char.rs
	clone.rs
	cmp.rs
	default.rs
	finally.rs
	intrinstic.rs	// 编译器固有函数，例如内存访问控制：Acquire, Release
	iter.rs
	kinds.rs
	mem.rs
	nonzero.rs
	ops.rs
	option.rs
	panicking.rs	// panic 
	prelude.rs
	ptr.rs
	raw.rs
	result.rs
	simd.rs
	slice.rs
	tuple.rs
	ty.rs

libcoretest

libflate
	
	zip 压缩

libfmt_macros

libgetopts

libgraphiviz

liblibc

liblog

librand

librbml

libregex

libregex_macros

librustc

librustc_back

librustc_borrowck

librustc_driver

librustc_llvm

librustc_resolve

librustc_trans

librustc_typeck

librustdoc

libserialize

libstd

libsyntax

libterm

libtest

libtime

libunicode

llvm

rt

rust-installer

rustllvm

	llvm 相关调用

test

