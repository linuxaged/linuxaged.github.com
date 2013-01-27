---
layout: post
title: "App Crack on Mac OSX"
description: ""
category: 
tags: []
---
{% include JB/setup %}
## 参考： http://loadcode.blogspot.co.uk/2006/02/cracking-software-on-os-x.html http://thexploit.com/secdev/mac-os-x-64-bit-assembly-system-calls/ http://shanewfx.github.com/blog/2012/03/26/vs2005-64bit-programming/

##TODO:
***
***
***
### 把二进制程序扔到 sublime text 2 中 切换到 hex 模式，我们猜测校验 license 的那个函数叫做 check* 之类的，于是以 check 关键字进行搜索
### 果然有个函数 checkLicense()
# 

### 利用 si 命令逐步运行，直至找到 objc_msgSend ()，这个函数意味着接下来要向某个 obj-c 对象传递参数了，
### 而且传递的参数保存在了寄存器里面，反汇编
 x64 结构一共有16个64bit通用寄存器.
 在调用函数时, 寄存器 %rbp, %rbx, 以及 %r12, %r13, %r14, %r15 属于 caller, 剩下的属于 callee.

### 在实施真正的破解之前有必要熟悉一下 x64 
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

Dump of assembler code for function main:
0x0000000100000ec0 <main+0>:	push   %rbp
0x0000000100000ec1 <main+1>:	mov    %rsp,%rbp
0x0000000100000ec4 <main+4>:	sub    $0x20,%rsp	; 开辟栈空间 32 
0x0000000100000ec8 <main+8>:	mov    $0x20,%eax	; func() 第一个参数 32
0x0000000100000ecd <main+13>:	mov    $0x40,%ecx	; func() 第二个参数 64
0x0000000100000ed2 <main+18>:	xorps  %xmm0,%xmm0	; 前4个参数将传入XMM0到XMM3的寄存器
0x0000000100000ed5 <main+21>:	movl   $0x0,-0x4(%rbp) ; 这三句
x0000000100000edc <main+28>:	mov    %edi,-0x8(%rbp)
0x0000000100000edf <main+31>:	mov    %rsi,-0x10(%rbp)
0x0000000100000ee3 <main+35>:	movsd  %xmm0,-0x18(%rbp)
0x0000000100000ee8 <main+40>:	mov    %eax,%edi
0x0000000100000eea <main+42>:	mov    %ecx,%esi
0x0000000100000eec <main+44>:	callq  0x100000ea0 <_Z4funcii>
0x0000000100000ef1 <main+49>:	lea    0x43(%rip),%rdi        # 0x100000f3b
0x0000000100000ef8 <main+56>:	cvtsi2sd %eax,%xmm0
0x0000000100000efc <main+60>:	movsd  %xmm0,-0x18(%rbp)
0x0000000100000f01 <main+65>:	mov    $0x0,%al
0x0000000100000f03 <main+67>:	callq  0x100000f18 <dyld_stub_printf>
0x0000000100000f08 <main+72>:	cvttsd2si -0x18(%rbp),%ecx
0x0000000100000f0d <main+77>:	mov    %eax,-0x1c(%rbp)
0x0000000100000f10 <main+80>:	mov    %ecx,%eax	
0x0000000100000f12 <main+82>:	add    $0x20,%rsp	; 回收栈空间
0x0000000100000f16 <main+86>:	pop    %rbp
0x0000000100000f17 <main+87>:	retq   
End of assembler dump.


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

	Dump of assembler code for function -[SSDocumentController checkLicense:]:
	0x00000001000313c5 <-[checkLicense:]+0>:	push   %rbp
	0x00000001000313c6 <-[checkLicense:]+1>:	mov    %rsp,%rbp
	0x00000001000313c9 <-[checkLicense:]+4>:	push   %r15
	0x00000001000313cb <-[checkLicense:]+6>:	push   %r14
	0x00000001000313cd <-[checkLicense:]+8>:	push   %r13
	0x00000001000313cf <-[checkLicense:]+10>:	push   %r12
	0x00000001000313d1 <-[checkLicense:]+12>:	push   %rbx
	0x00000001000313d2 <-[checkLicense:]+13>:	sub    $0x18,%rsp
	0x00000001000313d6 <-[checkLicense:]+17>:	mov    %rdi,-0x30(%rbp) // 从 rbp 偏移 -0x30 放入 rdi
	0x00000001000313da <-[checkLicense:]+21>:	mov    0x3e117(%rip),%r14        # 0x10006f4f8
	0x00000001000313e1 <-[checkLicense:]+28>:	mov    0x3e970(%rip),%rbx        # 0x10006fd58
	0x00000001000313e8 <-[checkLicense:]+35>:	mov    %rdx,%rdi
	0x00000001000313eb <-[checkLicense:]+38>:	callq  0x100040ee4 <dyld_stub_objc_retain>
	0x00000001000313f0 <-[checkLicense:]+43>:	mov    %rax,-0x38(%rbp)
	0x00000001000313f4 <-[checkLicense:]+47>:	mov    0x2cd5d(%rip),%r15        # 0x10005e158
	0x00000001000313fb <-[checkLicense:]+54>:	mov    %rbx,%rdi
	0x00000001000313fe <-[checkLicense:]+57>:	mov    %r14,%rsi
	0x0000000100031401 <-[checkLicense:]+60>:	callq  *%r15
	0x0000000100031404 <-[checkLicense:]+63>:	mov    %rax,%rdi
	0x0000000100031407 <-[checkLicense:]+66>:	callq  0x100040ef6 <dyld_stub_objc_retainAutoreleasedReturnValue>
	0x000000010003140c <-[checkLicense:]+71>:	mov    %rax,%rbx
	0x000000010003140f <-[checkLicense:]+74>:	lea    0x41c0a(%rip),%rdx        # 0x100073020
	0x0000000100031416 <-[checkLicense:]+81>:	mov    0x3d2c3(%rip),%rsi        # 0x10006e6e0
	0x000000010003141d <-[checkLicense:]+88>:	mov    %rbx,%rdi
	0x0000000100031420 <-[checkLicense:]+91>:	callq  *%r15
	0x0000000100031423 <-[checkLicense:]+94>:	lea    0x41c16(%rip),%r13        # 0x100073040
	0x000000010003142a <-[checkLicense:]+101>:	mov    0x3d2af(%rip),%rsi        # 0x10006e6e0
	0x0000000100031431 <-[checkLicense:]+108>:	mov    %rbx,%rdi
	0x0000000100031434 <-[checkLicense:]+111>:	mov    %r13,%rdx
	0x0000000100031437 <-[checkLicense:]+114>:	callq  *%r15
	0x000000010003143a <-[checkLicense:]+117>:	mov    0x3d29f(%rip),%rsi        # 0x10006e6e0
	0x0000000100031441 <-[checkLicense:]+124>:	mov    %rbx,%rdi
	0x0000000100031444 <-[checkLicense:]+127>:	mov    %r13,%rdx
	0x0000000100031447 <-[checkLicense:]+130>:	callq  *%r15
	0x000000010003144a <-[checkLicense:]+133>:	lea    0x41c0f(%rip),%rdx        # 0x100073060
	0x0000000100031451 <-[checkLicense:]+140>:	mov    0x3d288(%rip),%rsi        # 0x10006e6e0
	0x0000000100031458 <-[checkLicense:]+147>:	mov    %rbx,%rdi
	0x000000010003145b <-[checkLicense:]+150>:	callq  *%r15
	0x000000010003145e <-[checkLicense:]+153>:	lea    0x41c1b(%rip),%rdx        # 0x100073080
	0x0000000100031465 <-[checkLicense:]+160>:	mov    0x3d274(%rip),%rsi        # 0x10006e6e0
	0x000000010003146c <-[checkLicense:]+167>:	mov    %rbx,%rdi
	0x000000010003146f <-[checkLicense:]+170>:	callq  *%r15
	0x0000000100031472 <-[checkLicense:]+173>:	lea    0x41c27(%rip),%r12        # 0x1000730a0
	0x0000000100031479 <-[checkLicense:]+180>:	mov    0x3d260(%rip),%rsi        # 0x10006e6e0
	0x0000000100031480 <-[checkLicense:]+187>:	mov    %rbx,%rdi
	0x0000000100031483 <-[checkLicense:]+190>:	mov    %r12,%rdx
	0x0000000100031486 <-[checkLicense:]+193>:	callq  *%r15
	0x0000000100031489 <-[checkLicense:]+196>:	mov    0x3d250(%rip),%rsi        # 0x10006e6e0
	0x0000000100031490 <-[checkLicense:]+203>:	mov    %rbx,%rdi
	0x0000000100031493 <-[checkLicense:]+206>:	mov    %r12,%rdx
	0x0000000100031496 <-[checkLicense:]+209>:	callq  *%r15
	0x0000000100031499 <-[checkLicense:]+212>:	lea    0x41c20(%rip),%rdx        # 0x1000730c0
	0x00000001000314a0 <-[checkLicense:]+219>:	mov    0x3d239(%rip),%rsi        # 0x10006e6e0
	0x00000001000314a7 <-[checkLicense:]+226>:	mov    %rbx,%rdi
	0x00000001000314aa <-[checkLicense:]+229>:	callq  *%r15
	0x00000001000314ad <-[checkLicense:]+232>:	lea    0x41c2c(%rip),%rdx        # 0x1000730e0
	0x00000001000314b4 <-[checkLicense:]+239>:	mov    0x3d225(%rip),%rsi        # 0x10006e6e0
	0x00000001000314bb <-[checkLicense:]+246>:	mov    %rbx,%rdi
	0x00000001000314be <-[checkLicense:]+249>:	callq  *%r15
	0x00000001000314c1 <-[checkLicense:]+252>:	lea    0x41c38(%rip),%r14        # 0x100073100
	0x00000001000314c8 <-[checkLicense:]+259>:	mov    0x3d211(%rip),%rsi        # 0x10006e6e0
	0x00000001000314cf <-[checkLicense:]+266>:	mov    %rbx,%rdi
	0x00000001000314d2 <-[checkLicense:]+269>:	mov    %r14,%rdx
	0x00000001000314d5 <-[checkLicense:]+272>:	callq  *%r15
	0x00000001000314d8 <-[checkLicense:]+275>:	mov    0x3d201(%rip),%rsi        # 0x10006e6e0
	0x00000001000314df <-[checkLicense:]+282>:	mov    %rbx,%rdi
	0x00000001000314e2 <-[checkLicense:]+285>:	mov    %r14,%rdx
	0x00000001000314e5 <-[checkLicense:]+288>:	callq  *%r15
	0x00000001000314e8 <-[checkLicense:]+291>:	lea    0x41c31(%rip),%rdx        # 0x100073120
	0x00000001000314ef <-[checkLicense:]+298>:	mov    0x3d1ea(%rip),%rsi        # 0x10006e6e0
	0x00000001000314f6 <-[checkLicense:]+305>:	mov    %rbx,%rdi
	0x00000001000314f9 <-[checkLicense:]+308>:	callq  *%r15
	0x00000001000314fc <-[checkLicense:]+311>:	lea    0x41c3d(%rip),%rdx        # 0x100073140
	0x0000000100031503 <-[checkLicense:]+318>:	mov    0x3d1d6(%rip),%rsi        # 0x10006e6e0
	0x000000010003150a <-[checkLicense:]+325>:	mov    %rbx,%rdi
	0x000000010003150d <-[checkLicense:]+328>:	callq  *%r15
	0x0000000100031510 <-[checkLicense:]+331>:	mov    0x3d1c9(%rip),%rsi        # 0x10006e6e0
	0x0000000100031517 <-[checkLicense:]+338>:	mov    %rbx,%rdi
	0x000000010003151a <-[checkLicense:]+341>:	mov    %r13,%rdx
	0x000000010003151d <-[checkLicense:]+344>:	callq  *%r15
	0x0000000100031520 <-[checkLicense:]+347>:	mov    0x3d1b9(%rip),%rsi        # 0x10006e6e0
	0x0000000100031527 <-[checkLicense:]+354>:	mov    %rbx,%rdi
	0x000000010003152a <-[checkLicense:]+357>:	mov    %r13,%rdx
	0x000000010003152d <-[checkLicense:]+360>:	callq  *%r15
	0x0000000100031530 <-[checkLicense:]+363>:	lea    0x41c29(%rip),%rdx        # 0x100073160
	0x0000000100031537 <-[checkLicense:]+370>:	mov    0x3d1a2(%rip),%rsi        # 0x10006e6e0
	0x000000010003153e <-[checkLicense:]+377>:	mov    %rbx,%rdi
	0x0000000100031541 <-[checkLicense:]+380>:	callq  *%r15
	0x0000000100031544 <-[checkLicense:]+383>:	lea    0x41c35(%rip),%rdx        # 0x100073180
	0x000000010003154b <-[checkLicense:]+390>:	mov    0x3d18e(%rip),%rsi        # 0x10006e6e0
	0x0000000100031552 <-[checkLicense:]+397>:	mov    %rbx,%rdi
	0x0000000100031555 <-[checkLicense:]+400>:	callq  *%r15
	0x0000000100031558 <-[checkLicense:]+403>:	lea    0x41c41(%rip),%r13        # 0x1000731a0
	0x000000010003155f <-[checkLicense:]+410>:	mov    0x3d17a(%rip),%rsi        # 0x10006e6e0
	0x0000000100031566 <-[checkLicense:]+417>:	mov    %rbx,%rdi
	0x0000000100031569 <-[checkLicense:]+420>:	mov    %r13,%rdx
	0x000000010003156c <-[checkLicense:]+423>:	callq  *%r15
	0x000000010003156f <-[checkLicense:]+426>:	mov    0x3d16a(%rip),%rsi        # 0x10006e6e0
	0x0000000100031576 <-[checkLicense:]+433>:	mov    %rbx,%rdi
	0x0000000100031579 <-[checkLicense:]+436>:	mov    %r13,%rdx
	0x000000010003157c <-[checkLicense:]+439>:	callq  *%r15
	0x000000010003157f <-[checkLicense:]+442>:	lea    0x41c3a(%rip),%rdx        # 0x1000731c0
	0x0000000100031586 <-[checkLicense:]+449>:	mov    0x3d153(%rip),%rsi        # 0x10006e6e0
	0x000000010003158d <-[checkLicense:]+456>:	mov    %rbx,%rdi
	0x0000000100031590 <-[checkLicense:]+459>:	callq  *%r15
	0x0000000100031593 <-[checkLicense:]+462>:	lea    0x41c46(%rip),%rdx        # 0x1000731e0
	0x000000010003159a <-[checkLicense:]+469>:	mov    0x3d13f(%rip),%rsi        # 0x10006e6e0
	0x00000001000315a1 <-[checkLicense:]+476>:	mov    %rbx,%rdi
	0x00000001000315a4 <-[checkLicense:]+479>:	callq  *%r15
	0x00000001000315a7 <-[checkLicense:]+482>:	mov    0x3d132(%rip),%rsi        # 0x10006e6e0
	0x00000001000315ae <-[checkLicense:]+489>:	mov    %rbx,%rdi
	0x00000001000315b1 <-[checkLicense:]+492>:	mov    %r12,%rdx
	0x00000001000315b4 <-[checkLicense:]+495>:	callq  *%r15
	0x00000001000315b7 <-[checkLicense:]+498>:	mov    0x3d122(%rip),%rsi        # 0x10006e6e0
	0x00000001000315be <-[checkLicense:]+505>:	mov    %rbx,%rdi
	0x00000001000315c1 <-[checkLicense:]+508>:	mov    %r12,%rdx
	0x00000001000315c4 <-[checkLicense:]+511>:	callq  *%r15
	0x00000001000315c7 <-[checkLicense:]+514>:	lea    0x41c32(%rip),%rdx        # 0x100073200
	0x00000001000315ce <-[checkLicense:]+521>:	mov    0x3d10b(%rip),%rsi        # 0x10006e6e0
	0x00000001000315d5 <-[checkLicense:]+528>:	mov    %rbx,%rdi
	0x00000001000315d8 <-[checkLicense:]+531>:	callq  *%r15
	0x00000001000315db <-[checkLicense:]+534>:	lea    0x41c3e(%rip),%rdx        # 0x100073220
	0x00000001000315e2 <-[checkLicense:]+541>:	mov    0x3d0f7(%rip),%rsi        # 0x10006e6e0
	0x00000001000315e9 <-[checkLicense:]+548>:	mov    %rbx,%rdi
	0x00000001000315ec <-[checkLicense:]+551>:	callq  *%r15
	0x00000001000315ef <-[checkLicense:]+554>:	lea    0x41c4a(%rip),%rdx        # 0x100073240
	0x00000001000315f6 <-[checkLicense:]+561>:	mov    0x3d0e3(%rip),%rsi        # 0x10006e6e0
	0x00000001000315fd <-[checkLicense:]+568>:	mov    %rbx,%rdi
	0x0000000100031600 <-[checkLicense:]+571>:	callq  *%r15
	0x0000000100031603 <-[checkLicense:]+574>:	mov    0x3d0d6(%rip),%rsi        # 0x10006e6e0
	0x000000010003160a <-[checkLicense:]+581>:	mov    %rbx,%rdi
	0x000000010003160d <-[checkLicense:]+584>:	mov    %r13,%rdx
	0x0000000100031610 <-[checkLicense:]+587>:	callq  *%r15
	0x0000000100031613 <-[checkLicense:]+590>:	mov    0x3d0c6(%rip),%rsi        # 0x10006e6e0
	0x000000010003161a <-[checkLicense:]+597>:	mov    %rbx,%rdi
	0x000000010003161d <-[checkLicense:]+600>:	mov    %r13,%rdx
	0x0000000100031620 <-[checkLicense:]+603>:	callq  *%r15
	0x0000000100031623 <-[checkLicense:]+606>:	lea    0x41c36(%rip),%rdx        # 0x100073260
	0x000000010003162a <-[checkLicense:]+613>:	mov    0x3d0af(%rip),%rsi        # 0x10006e6e0
	0x0000000100031631 <-[checkLicense:]+620>:	mov    %rbx,%rdi
	0x0000000100031634 <-[checkLicense:]+623>:	callq  *%r15
	0x0000000100031637 <-[checkLicense:]+626>:	lea    0x41c42(%rip),%rdx        # 0x100073280
	0x000000010003163e <-[checkLicense:]+633>:	mov    0x3d09b(%rip),%rsi        # 0x10006e6e0
	0x0000000100031645 <-[checkLicense:]+640>:	mov    %rbx,%rdi
	0x0000000100031648 <-[checkLicense:]+643>:	callq  *%r15
	0x000000010003164b <-[checkLicense:]+646>:	mov    0x3deae(%rip),%rsi        # 0x10006f500
	0x0000000100031652 <-[checkLicense:]+653>:	mov    0x3e827(%rip),%rdi        # 0x10006fe80
	0x0000000100031659 <-[checkLicense:]+660>:	mov    %rbx,%rdx
	0x000000010003165c <-[checkLicense:]+663>:	callq  *%r15
	0x000000010003165f <-[checkLicense:]+666>:	mov    %rax,%rdi
	0x0000000100031662 <-[checkLicense:]+669>:	callq  0x100040ef6 <dyld_stub_objc_retainAutoreleasedReturnValue>
	0x0000000100031667 <-[checkLicense:]+674>:	mov    %rax,%r12
	0x000000010003166a <-[checkLicense:]+677>:	mov    0x3de97(%rip),%rsi        # 0x10006f508
	0x0000000100031671 <-[checkLicense:]+684>:	mov    %r12,%rdi
	0x0000000100031674 <-[checkLicense:]+687>:	mov    -0x38(%rbp),%r13
	0x0000000100031678 <-[checkLicense:]+691>:	mov    %r13,%rdx
	0x000000010003167b <-[checkLicense:]+694>:	callq  *%r15
	0x000000010003167e <-[checkLicense:]+697>:	mov    %rax,%r14
	0x0000000100031681 <-[checkLicense:]+700>:	mov    %r13,%rdi
	0x0000000100031684 <-[checkLicense:]+703>:	callq  0x100040ede <dyld_stub_objc_release>
	0x0000000100031689 <-[checkLicense:]+708>:	mov    %r14,%rdi
	0x000000010003168c <-[checkLicense:]+711>:	callq  0x100040ef6 <dyld_stub_objc_retainAutoreleasedReturnValue>
	0x0000000100031691 <-[checkLicense:]+716>:	mov    %rax,%r14
	0x0000000100031694 <-[checkLicense:]+719>:	mov    0x3dd6d(%rip),%rsi        # 0x10006f408
	0x000000010003169b <-[checkLicense:]+726>:	mov    -0x30(%rbp),%r13
	0x000000010003169f <-[checkLicense:]+730>:	mov    %r13,%rdi
	0x00000001000316a2 <-[checkLicense:]+733>:	mov    %r14,%rdx
	0x00000001000316a5 <-[checkLicense:]+736>:	callq  *%r15
	0x00000001000316a8 <-[checkLicense:]+739>:	mov    %r14,%rdi
	0x00000001000316ab <-[checkLicense:]+742>:	callq  0x100040ede <dyld_stub_objc_release>
	0x00000001000316b0 <-[checkLicense:]+747>:	mov    0x3de59(%rip),%rsi        # 0x10006f510
	0x00000001000316b7 <-[checkLicense:]+754>:	mov    %r12,%rdi
	0x00000001000316ba <-[checkLicense:]+757>:	callq  *%r15
	0x00000001000316bd <-[checkLicense:]+760>:	mov    %rax,%rdi
	0x00000001000316c0 <-[checkLicense:]+763>:	callq  0x100040ef6 <dyld_stub_objc_retainAutoreleasedReturnValue>
	0x00000001000316c5 <-[checkLicense:]+768>:	mov    %rax,%r14
	0x00000001000316c8 <-[checkLicense:]+771>:	mov    0x3de49(%rip),%rsi        # 0x10006f518
	0x00000001000316cf <-[checkLicense:]+778>:	mov    %r13,%rdi
	0x00000001000316d2 <-[checkLicense:]+781>:	mov    %r14,%rdx
	0x00000001000316d5 <-[checkLicense:]+784>:	callq  *%r15
	0x00000001000316d8 <-[checkLicense:]+787>:	mov    %r14,%rdi
	0x00000001000316db <-[checkLicense:]+790>:	callq  0x100040ede <dyld_stub_objc_release>
	0x00000001000316e0 <-[checkLicense:]+795>:	mov    %r12,%rdi
	0x00000001000316e3 <-[checkLicense:]+798>:	callq  0x100040ede <dyld_stub_objc_release>
	0x00000001000316e8 <-[checkLicense:]+803>:	mov    %rbx,%rdi
	0x00000001000316eb <-[checkLicense:]+806>:	add    $0x18,%rsp
	0x00000001000316ef <-[checkLicense:]+810>:	pop    %rbx
	0x00000001000316f0 <-[checkLicense:]+811>:	pop    %r12
	0x00000001000316f2 <-[checkLicense:]+813>:	pop    %r13
	0x00000001000316f4 <-[checkLicense:]+815>:	pop    %r14
	0x00000001000316f6 <-[checkLicense:]+817>:	pop    %r15
	0x00000001000316f8 <-[checkLicense:]+819>:	pop    %rbp
	0x00000001000316f9 <-[checkLicense:]+820>:	jmpq   0x100040ede <dyld_stub_objc_release>
	End of assembler dump.
# 

	(gdb) info all-registers
	rax            0x101b242b0	4323426992
	rbx            0x7fff7a8df700	140735249512192
	rcx            0x100800908	4303358216
	rdx            0x0	0
	rsi            0x7fff9278a284	140735650767492
	rdi            0x7fff7a8df700	140735249512192
	rbp            0x7fff5fbff250	0x7fff5fbff250
	rsp            0x7fff5fbff208	0x7fff5fbff208
	r8             0xfffffffefe4dbd4f	-4323426993
	r9             0x3f	63
	r10            0x100800800	4303357952
	r11            0xf9bfd10	261881104
	r12            0x7fff961a9240	140735711711808
	r13            0x0	0
	r14            0x7fff9278a284	140735650767492
	r15            0x7fff961a9240	140735711711808
	rip            0x7fff961a9240	0x7fff961a9240 <objc_msgSend>
	eflags         0x202	514
	cs             0x2b	43
	ss             0x0	0
	ds             0x0	0
	es             0x0	0
	fs             0x0	0
	gs             0x0	0
	st0            <invalid float value>	(raw 0xffff0000000000000000)
	st1            -nan(0x000000100)	(raw 0xffff0000000000000100)
	st2            0	(raw 0x00000000000000000000)
	st3            -nan(0x00000001a)	(raw 0xffff000000000000001a)
	st4            -nan(0x0ffe96500)	(raw 0xffff00000000ffe96500)
	st5            -9223372031854775808	(raw 0xc03dfffffffdabf41c00)
	st6            5000000000	(raw 0x401f9502f90000000000)
	st7            5000000000	(raw 0x401f9502f90000000000)
	fctrl          0x37f	895
	fstat          0x0	0
	ftag           0xffff	65535
	fiseg          0x2b	43
	fioff          0x8aae339d	-1968295011
	foseg          0x23	35
	fooff          0x5fbfba50	1606400592
	fop            0x0	0
	xmm0           {
	  v4_float = {-nan(0x7fffff), -nan(0x7fffff), -nan(0x7f0000), 0}, 
	  v2_double = {-nan(0xfffffffffffff), -nan(0xf000000000000)}, 
	  v16_int8 = {-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, 0, 0, 0, 0, 0, 0}, 
	  v8_int16 = {-1, -1, -1, -1, -1, 0, 0, 0}, 
	  v4_int32 = {-1, -1, -65536, 0}, 
	  v2_int64 = {-1, -281474976710656}, 
	  uint128 = 0xffffffffffffffffffff000000000000
	}	(raw 0x000000000000ffffffffffffffffffff)
	xmm1           {
	  v4_float = {7.14284101e+31, 2.75614803e+23, 907120832, 9.25445743e+18}, 
	  v2_double = {3.9838610162521671e+252, 2.6177531811662865e+69}, 
	  v16_int8 = {116, 97, 99, 105, 102, 105, 116, 111, 78, 88, 70, 67, 95, 0, 110, 112}, 
	  v8_int16 = {29793, 25449, 26217, 29807, 20056, 17987, 24320, 28272}, 
	  v4_int32 = {1952539497, 1718187119, 1314408003, 1593863792}, 
	  v2_int64 = {8386093285481477231, 5645339388079533680}, 
	  uint128 = 0x746163696669746f4e5846435f006e70
	}	(raw 0x706e005f4346584e6f74696669636174)
	xmm2           {
	  v4_float = {0, 0, 0, 0}, 
	  v2_double = {0, 0}, 
	  v16_int8 = {0 <repeats 16 times>}, 
	  v8_int16 = {0, 0, 0, 0, 0, 0, 0, 0}, 
	  v4_int32 = {0, 0, 0, 0}, 
	  v2_int64 = {0, 0}, 
	  uint128 = 0
	}	(raw 0x00000000000000000000000000000000)
	xmm3           {
	  v4_float = {1.40129846e-45, 4.37701181e-40, 1.40129846e-45, 2.84518239e-40}, 
	  v2_double = {2.1221501143460134e-314, 2.1220961055599383e-314}, 
	  v16_int8 = {0, 0, 0, 1, 0, 4, -60, 34, 0, 0, 0, 1, 0, 3, 25, 31}, 
	  v8_int16 = {0, 1, 4, -15326, 0, 1, 3, 6431}, 
	  v4_int32 = {1, 312354, 1, 203039}, 
	  v2_int64 = {4295279650, 4295170335}, 
	  uint128 = 0x000000010004c422000000010003191f
	}	(raw 0x1f1903000100000022c4040001000000)
	xmm4           {
	  v4_float = {0, 0, -1.875, 0}, 
	  v2_double = {0, -1}, 
	  v16_int8 = {0, 0, 0, 0, 0, 0, 0, 0, -65, -16, 0, 0, 0, 0, 0, 0}, 
	  v8_int16 = {0, 0, 0, 0, -16400, 0, 0, 0}, 
	  v4_int32 = {0, 0, -1074790400, 0}, 
	  v2_int64 = {0, -4616189618054758400}, 
	  uint128 = 61631
	}	(raw 0x000000000000f0bf0000000000000000)
	xmm5           {
	  v4_float = {0, 0, 0, 0}, 
	  v2_double = {0, 0}, 
	  v16_int8 = {0 <repeats 16 times>}, 
	  v8_int16 = {0, 0, 0, 0, 0, 0, 0, 0}, 
	  v4_int32 = {0, 0, 0, 0}, 
	  v2_int64 = {0, 0}, 
	  uint128 = 0
	}	(raw 0x00000000000000000000000000000000)
	xmm6           {
	  v4_float = {0, 0, 0, 0}, 
	  v2_double = {0, 0}, 
	  v16_int8 = {0 <repeats 16 times>}, 
	  v8_int16 = {0, 0, 0, 0, 0, 0, 0, 0}, 
	  v4_int32 = {0, 0, 0, 0}, 
	  v2_int64 = {0, 0}, 
	  uint128 = 0
	}	(raw 0x00000000000000000000000000000000)
	xmm7           {
	  v4_float = {0, 0, 0, 0}, 
	  v2_double = {0, 0}, 
	  v16_int8 = {0 <repeats 16 times>}, 
	  v8_int16 = {0, 0, 0, 0, 0, 0, 0, 0}, 
	  v4_int32 = {0, 0, 0, 0}, 
	  v2_int64 = {0, 0}, 
	  uint128 = 0
	}	(raw 0x00000000000000000000000000000000)
	xmm8           {
	  v4_float = {0, 0, 0, 0}, 
	  v2_double = {0, 0}, 
	  v16_int8 = {0 <repeats 16 times>}, 
	  v8_int16 = {0, 0, 0, 0, 0, 0, 0, 0}, 
	  v4_int32 = {0, 0, 0, 0}, 
	  v2_int64 = {0, 0}, 
	  uint128 = 0
	}	(raw 0x00000000000000000000000000000000)
	xmm9           {
	  v4_float = {0, 0, 0, 0}, 
	  v2_double = {0, 0}, 
	  v16_int8 = {0 <repeats 16 times>}, 
	  v8_int16 = {0, 0, 0, 0, 0, 0, 0, 0}, 
	  v4_int32 = {0, 0, 0, 0}, 
	  v2_int64 = {0, 0}, 
	  uint128 = 0
	}	(raw 0x00000000000000000000000000000000)
	xmm10          {
	  v4_float = {0, 0, 0, 0}, 
	  v2_double = {0, 0}, 
	  v16_int8 = {0 <repeats 16 times>}, 
	  v8_int16 = {0, 0, 0, 0, 0, 0, 0, 0}, 
	  v4_int32 = {0, 0, 0, 0}, 
	  v2_int64 = {0, 0}, 
	  uint128 = 0
	}	(raw 0x00000000000000000000000000000000)
	xmm11          {
	  v4_float = {0, 0, 0, 0}, 
	  v2_double = {0, 0}, 
	  v16_int8 = {0 <repeats 16 times>}, 
	  v8_int16 = {0, 0, 0, 0, 0, 0, 0, 0}, 
	  v4_int32 = {0, 0, 0, 0}, 
	  v2_int64 = {0, 0}, 
	  uint128 = 0
	}	(raw 0x00000000000000000000000000000000)
	xmm12          {
	  v4_float = {0, 0, 0, 0}, 
	  v2_double = {0, 0}, 
	  v16_int8 = {0 <repeats 16 times>}, 
	  v8_int16 = {0, 0, 0, 0, 0, 0, 0, 0}, 
	  v4_int32 = {0, 0, 0, 0}, 
	  v2_int64 = {0, 0}, 
	  uint128 = 0
	}	(raw 0x00000000000000000000000000000000)
	xmm13          {
	  v4_float = {0, 0, 0, 0}, 
	  v2_double = {0, 0}, 
	  v16_int8 = {0 <repeats 16 times>}, 
	  v8_int16 = {0, 0, 0, 0, 0, 0, 0, 0}, 
	  v4_int32 = {0, 0, 0, 0}, 
	  v2_int64 = {0, 0}, 
	  uint128 = 0
	}	(raw 0x00000000000000000000000000000000)
	xmm14          {
	  v4_float = {0, 0, 0, 0}, 
	  v2_double = {0, 0}, 
	  v16_int8 = {0 <repeats 16 times>}, 
	  v8_int16 = {0, 0, 0, 0, 0, 0, 0, 0}, 
	  v4_int32 = {0, 0, 0, 0}, 
	  v2_int64 = {0, 0}, 
	  uint128 = 0
	}	(raw 0x00000000000000000000000000000000)
	xmm15          {
	  v4_float = {0, 0, 0, 0}, 
	  v2_double = {0, 0}, 
	  v16_int8 = {0 <repeats 16 times>}, 
	  v8_int16 = {0, 0, 0, 0, 0, 0, 0, 0}, 
	  v4_int32 = {0, 0, 0, 0}, 
	  v2_int64 = {0, 0}, 
	  uint128 = 0
	}	(raw 0x00000000000000000000000000000000)
	mxcsr          0x1fa0	8096


