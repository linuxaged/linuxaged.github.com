---
layout: post
title: "local vs. global variable"
description: ""
category: 
tags: []
---
{% include JB/setup %}
栈帧的形成和关闭
In any language local variables will be faster. You will need to understand what a register is and what the thread stack is to understand my explanation. Most local variables are implemented as a register variable or pushed near the top of the local stack, so they are generally accessed much more quickly. Global variables are stored further up the stack (if they are not on the heap) so computing their address to access them is slower.


Global variable = global memory, and subject to aliasing (read as: bad for the optimizer -- must read-modify-write in the worst case).

Local variable = register (unless the compiler really can't help it, sometimes it must put it on the stack too, but the stack is practically guaranteed to be in L1)

Accessing a register is on the order of a single cycle, accessing memory is on the order of 15-1000 cycles (depending on whether the cache line is in cache and not invalidated by another core, and depending on whether the page is in the TLB