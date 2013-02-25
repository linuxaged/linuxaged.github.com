---
layout: post
title: "App Crack on Mac OSX"
description: ""
category: 
tags: []
---
{% include JB/setup %}
#参考
http://lifehacker.com/5736101/how-to-crack-just-about-any-mac-app-and-how-to-prevent-it

http://loadcode.blogspot.co.uk/2006/02/cracking-software-on-os-x.html

***
#基础
###X64汇编
汇编一个简单程序:

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

#步骤
这里以一款xxx应用为例,xxx应用未注册时无法使用导出功能

1.利用 classdump 找出我们要crack的函数

	class-dump executable | mate

搜索关键字 export, 初步确定为 MyDocument 类中的 export 方法

2.利用 otx 找出段和偏移

	otx xxx -arch i386 | mate

搜索关键字 export,找到方法 [MyDocument export:]

	-[MyDocument export:]:
	    +0  0000000100007678  55                        pushq       %rbp
	    +1  0000000100007679  4889e5                    movq        %rsp,                         %rbp
	    +4  000000010000767c  4157                      pushq       %r15
	    +6  000000010000767e  4156                      pushq       %r14
	    +8  0000000100007680  4155                      pushq       %r13
	   +10  0000000100007682  4154                      pushq       %r12
	   +12  0000000100007684  53                        pushq       %rbx
	   +13  0000000100007685  4883ec48                  subq        $72,                          %rsp
	   +17  0000000100007689  4989ff                    movq        %rdi,                         %r15
	   +20  000000010000768c  488b3525660600            movq        419365(%rip),                 %rsi
	   +27  0000000100007693  ff15bf6a0500              callq       *355007(%rip)                 -[%rdi appRegistered]
	   +33  0000000100007699  84c0                      testb       %al,                          %al
	   +35  000000010000769b  0f85a5000000              jne         0x100007746

3.记录 jne 和 je 指令的二进制码

	0f8427010000              je
	0f85a5000000              jne

4.切换到要破解的应用的二进制文件所在目录

		$cd /Applications/xxx.app/Contents/MacOS

5..启动gdb调试器,载入二进制文件

		$gdb xxx

6.找到 export 函数,打断点 ; 运行

		(gdb)b export
		(gdb)run
		(gdb) x/x 0x000000010000769b
		0x10000769b <-[MyDocument export:]+35>:	0x00a5850f
		(gdb) x/x 0x000000010000769c
		0x10000769c <-[MyDocument export:]+36>:	0x0000a585

7.最后我们发现 把 0x10000769c处的 85 改成 84 就行了