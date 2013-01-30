---
layout: post
title: "App Crack on Mac OSX"
description: ""
category: 
tags: []
---
{% include JB/setup %}
## 参考： 
(http://loadcode.blogspot.co.uk/2006/02/cracking-software-on-os-x.html)
(http://thexploit.com/secdev/mac-os-x-64-bit-assembly-system-calls/)
(http://shanewfx.github.com/blog/2012/03/26/vs2005-64bit-programming/)
(http://en.wikipedia.org/wiki/X86_calling_conventions)
(http://eli.thegreenplace.net/2011/09/06/stack-frame-layout-on-x86-64/)
(http://www.uclibc.org/docs/psABI-x86_64.pdf)

***
##TODO:##
jne, je 在内存里的表示
***

### 把二进制程序扔到 sublime text 2 中 切换到 hex 模式，我们猜测校验 license 的那个函数叫做 check* 之类的，于是以 check 关键字进行搜索
### 果然有个函数 checkLicense()
***
### 利用 si 命令逐步运行，直至找到 objc_msgSend ()，这个函数意味着接下来要向某个 obj-c 对象传递参数了，
### 而且传递的参数保存在了寄存器里面，反汇编
 x64 结构一共有16个64bit通用寄存器.
 在调用函数时, 寄存器 %rbp, %rbx, 以及 %r12, %r13, %r14, %r15 属于 caller, 剩下的属于 callee.

### x64 calling convention
在64的世界里有两个 calling convention 标准，一个 Windows, 一个 Unix. 这里只关注 Unix 的.
####Intel 处理器从 32 到 64 位 寄存器的变化：
	
	拓展的寄存器	
	| 32-bit | EAX | EBX | ECX | EDX | ESI | EDI | EBP | ESP |
	|--------|-----|-----|-----|-----|-----|-----|-----|-----|
	| 64-bit | RAX | RBX | RCX | RDX | RSI | RDI | RBP | RSP |
	
    增加的寄存器
	R8 ~ R15, XMM8 ~ XMM15

####Intel 处理器从 32 到 64 位函数调用的变化：
	1.前4个整数参数（从左至右）通过4个寄存器传递：RCX RDX R8 R9
	2.前4个以外的整数参数将传递到堆栈
	3.指针被视为整数参数
	4.对于浮点参数，前4个参数将传入XMM0到XMM3的寄存器，后续的浮点参数通过堆栈传递。

即使参数可以是通过寄存器传递，但在堆栈上仍需为其预留空间，每个函数至少要在堆栈上预留32个字节（为前4个参数预留空间）, 
该空间允许将传递函数函数的寄存器轻松地复制到堆栈中。
当然，如果要传递4个以上的参数，则必须为其预额外的堆栈空间。
调用者负责椎栈空间的分配与回收，被调用函数不需要自己负责平衡堆栈（仅用于传递参数的这部分堆栈空间）
注意，callee中有局部变量和保存其他寄存器时，其空间是由被调用函数来分配，并在结束时由自己去回收这部分堆栈空间
####汇编一个简单程序:


	int func(int a, int b)
	{
	    return a*b;
	}
	int main(int argc, const char * argv[])
	{
	    double returnValue = 0;
	    returnValue = func(32, 64);
	    printf("Hello World\n");
	    return returnValue;
	}

对应的反汇编结果：

	Main 函数：
	Dump of assembler code for function main:
	0x0000000100000ef0 <main+0>:	push   %rbp
	0x0000000100000ef1 <main+1>:	mov    %rsp,%rbp
	0x0000000100000ef4 <main+4>:	sub    $0x20,%rsp	; 开辟栈空间 32 
	0x0000000100000ef8 <main+8>:	mov    $0x20,%eax   ; func() 第一个参数 32
	0x0000000100000efd <main+13>:	mov    $0x40,%ecx   ; func() 第二个参数 64
	0x0000000100000f02 <main+18>:	movl   $0x0,-0x4(%rbp)	; 雷打不动的三行
	0x0000000100000f09 <main+25>:	mov    %edi,-0x8(%rbp)	; 雷打不动的三行
	0x0000000100000f0c <main+28>:	mov    %rsi,-0x10(%rbp)	; 雷打不动的三行
	0x0000000100000f10 <main+32>:	movl   $0x0,-0x14(%rbp)	; 两个参数入栈
	0x0000000100000f17 <main+39>:	movl   $0x0,-0x18(%rbp)
	0x0000000100000f1e <main+46>:	mov    %eax,%edi	; edi,esi 用作真正的参数寄存器
	0x0000000100000f20 <main+48>:	mov    %ecx,%esi
	0x0000000100000f22 <main+50>:	callq  0x100000ed0 <_Z4funcii>
	0x0000000100000f27 <main+55>:	mov    $0x8,%edi	; 第二次 调用func() 的参数
	0x0000000100000f2c <main+60>:	mov    $0x10,%esi
	0x0000000100000f31 <main+65>:	mov    %eax,-0x14(%rbp)	; 第一次 调用 func() 结果入栈
	0x0000000100000f34 <main+68>:	callq  0x100000ed0 <_Z4funcii>
	0x0000000100000f39 <main+73>:	mov    %eax,-0x18(%rbp)
	0x0000000100000f3c <main+76>:	mov    -0x18(%rbp),%eax
	0x0000000100000f3f <main+79>:	add    $0x20,%rsp		; 收回栈空间
	0x0000000100000f43 <main+83>:	pop    %rbp
	0x0000000100000f44 <main+84>:	retq   
	End of assembler dump.
	
	******************************************************************
	
	func() 函数
	Dump of assembler code for function _Z4funcii:
	0x0000000100000ed0 <_Z4funcii+0>:	push   %rbp
	0x0000000100000ed1 <_Z4funcii+1>:	mov    %rsp,%rbp
	0x0000000100000ed4 <_Z4funcii+4>:	mov    %edi,-0x4(%rbp)
	0x0000000100000ed7 <_Z4funcii+7>:	mov    %esi,-0x8(%rbp)
	0x0000000100000eda <_Z4funcii+10>:	mov    -0x4(%rbp),%esi
	0x0000000100000edd <_Z4funcii+13>:	imul   -0x8(%rbp),%esi
	0x0000000100000ee1 <_Z4funcii+17>:	mov    %esi,%eax
	0x0000000100000ee3 <_Z4funcii+19>:	pop    %rbp
	0x0000000100000ee4 <_Z4funcii+20>:	retq   
	End of assembler dump.

***

***

	-[MyDocument export:]:
	...
	   +33  0000000100007699  84c0                      testb       %al,%al
	   +35  000000010000769b  0f85a5000000              jne         0x100007746
	   +41  00000001000076a1  488b35d86c0600            movq        0x00066cd8(%rip),%rsi         alertWithMessageText:defaultButton:alternateButton:otherButton:informativeTextWithFormat:
	   +48  00000001000076a8  488b3de1860600            movq        0x000686e1(%rip),%rdi
	   +55  00000001000076af  488d052aa30600            leaq        0x0006a32a(%rip),%rax         The export feature is only available in the registered version.

	   +35  000000010000769b  0f85a5000000              jne         0x100007746
	   +578  00000001000078ba  0f8427010000              je          0x1000079e7                   return;

好，我们考试找到这个 jne,

	(gdb) b export:
	Breakpoint 2 at 0x100007819
	(gdb) x/x 0x100007819
	0x100007819 <-[MyDocument export:]+17>:	0x48ff8949
	(gdb) x/x 0x10000782b
	0x10000782b <-[MyDocument export:]+35>:	0x00a5850f

