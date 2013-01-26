---
layout: post
title: "App Crack on Mac OSX"
description: ""
category: 
tags: []
---
{% include JB/setup %}
## 参考： http://loadcode.blogspot.co.uk/2006/02/cracking-software-on-os-x.html
# 

### 把二进制程序扔到 sublime text 2 中 切换到 hex 模式，我们猜测校验 license 的那个函数叫做 check* 之类的，于是以 check 关键字进行搜索
### 果然有个函数 checkLicense()
# 

### 利用 si 命令逐步运行，直至找到 objc_msgSend ()，这个函数意味着接下来要向某个 obj-c 对象传递参数了，
### 而且传递的参数保存在了寄存器里面，反汇编
	Dump of assembler code for function -[SSDocumentController checkLicense:]:
	0x00000001000313c5 <-[SSDocumentController checkLicense:]+0>:	push   %rbp
	0x00000001000313c6 <-[SSDocumentController checkLicense:]+1>:	mov    %rsp,%rbp
	0x00000001000313c9 <-[SSDocumentController checkLicense:]+4>:	push   %r15
	0x00000001000313cb <-[SSDocumentController checkLicense:]+6>:	push   %r14
	0x00000001000313cd <-[SSDocumentController checkLicense:]+8>:	push   %r13
	0x00000001000313cf <-[SSDocumentController checkLicense:]+10>:	push   %r12
	0x00000001000313d1 <-[SSDocumentController checkLicense:]+12>:	push   %rbx
	0x00000001000313d2 <-[SSDocumentController checkLicense:]+13>:	sub    $0x18,%rsp
	0x00000001000313d6 <-[SSDocumentController checkLicense:]+17>:	mov    %rdi,-0x30(%rbp)
	0x00000001000313da <-[SSDocumentController checkLicense:]+21>:	mov    0x3e117(%rip),%r14        # 0x10006f4f8
	0x00000001000313e1 <-[SSDocumentController checkLicense:]+28>:	mov    0x3e970(%rip),%rbx        # 0x10006fd58
	0x00000001000313e8 <-[SSDocumentController checkLicense:]+35>:	mov    %rdx,%rdi
	0x00000001000313eb <-[SSDocumentController checkLicense:]+38>:	callq  0x100040ee4 <dyld_stub_objc_retain>
	0x00000001000313f0 <-[SSDocumentController checkLicense:]+43>:	mov    %rax,-0x38(%rbp)
	0x00000001000313f4 <-[SSDocumentController checkLicense:]+47>:	mov    0x2cd5d(%rip),%r15        # 0x10005e158
	0x00000001000313fb <-[SSDocumentController checkLicense:]+54>:	mov    %rbx,%rdi
	0x00000001000313fe <-[SSDocumentController checkLicense:]+57>:	mov    %r14,%rsi
	0x0000000100031401 <-[SSDocumentController checkLicense:]+60>:	callq  *%r15
	0x0000000100031404 <-[SSDocumentController checkLicense:]+63>:	mov    %rax,%rdi
	0x0000000100031407 <-[SSDocumentController checkLicense:]+66>:	callq  0x100040ef6 <dyld_stub_objc_retainAutoreleasedReturnValue>
	0x000000010003140c <-[SSDocumentController checkLicense:]+71>:	mov    %rax,%rbx
	0x000000010003140f <-[SSDocumentController checkLicense:]+74>:	lea    0x41c0a(%rip),%rdx        # 0x100073020
	0x0000000100031416 <-[SSDocumentController checkLicense:]+81>:	mov    0x3d2c3(%rip),%rsi        # 0x10006e6e0
	0x000000010003141d <-[SSDocumentController checkLicense:]+88>:	mov    %rbx,%rdi
	0x0000000100031420 <-[SSDocumentController checkLicense:]+91>:	callq  *%r15
	0x0000000100031423 <-[SSDocumentController checkLicense:]+94>:	lea    0x41c16(%rip),%r13        # 0x100073040
	0x000000010003142a <-[SSDocumentController checkLicense:]+101>:	mov    0x3d2af(%rip),%rsi        # 0x10006e6e0
	0x0000000100031431 <-[SSDocumentController checkLicense:]+108>:	mov    %rbx,%rdi
	0x0000000100031434 <-[SSDocumentController checkLicense:]+111>:	mov    %r13,%rdx
	0x0000000100031437 <-[SSDocumentController checkLicense:]+114>:	callq  *%r15
	0x000000010003143a <-[SSDocumentController checkLicense:]+117>:	mov    0x3d29f(%rip),%rsi        # 0x10006e6e0
	0x0000000100031441 <-[SSDocumentController checkLicense:]+124>:	mov    %rbx,%rdi
	0x0000000100031444 <-[SSDocumentController checkLicense:]+127>:	mov    %r13,%rdx
	0x0000000100031447 <-[SSDocumentController checkLicense:]+130>:	callq  *%r15
	0x000000010003144a <-[SSDocumentController checkLicense:]+133>:	lea    0x41c0f(%rip),%rdx        # 0x100073060
	0x0000000100031451 <-[SSDocumentController checkLicense:]+140>:	mov    0x3d288(%rip),%rsi        # 0x10006e6e0
	0x0000000100031458 <-[SSDocumentController checkLicense:]+147>:	mov    %rbx,%rdi
	0x000000010003145b <-[SSDocumentController checkLicense:]+150>:	callq  *%r15
	0x000000010003145e <-[SSDocumentController checkLicense:]+153>:	lea    0x41c1b(%rip),%rdx        # 0x100073080
	0x0000000100031465 <-[SSDocumentController checkLicense:]+160>:	mov    0x3d274(%rip),%rsi        # 0x10006e6e0
	0x000000010003146c <-[SSDocumentController checkLicense:]+167>:	mov    %rbx,%rdi
	0x000000010003146f <-[SSDocumentController checkLicense:]+170>:	callq  *%r15
	0x0000000100031472 <-[SSDocumentController checkLicense:]+173>:	lea    0x41c27(%rip),%r12        # 0x1000730a0
	0x0000000100031479 <-[SSDocumentController checkLicense:]+180>:	mov    0x3d260(%rip),%rsi        # 0x10006e6e0
	0x0000000100031480 <-[SSDocumentController checkLicense:]+187>:	mov    %rbx,%rdi
	0x0000000100031483 <-[SSDocumentController checkLicense:]+190>:	mov    %r12,%rdx
	0x0000000100031486 <-[SSDocumentController checkLicense:]+193>:	callq  *%r15
	0x0000000100031489 <-[SSDocumentController checkLicense:]+196>:	mov    0x3d250(%rip),%rsi        # 0x10006e6e0
	0x0000000100031490 <-[SSDocumentController checkLicense:]+203>:	mov    %rbx,%rdi
	0x0000000100031493 <-[SSDocumentController checkLicense:]+206>:	mov    %r12,%rdx
	0x0000000100031496 <-[SSDocumentController checkLicense:]+209>:	callq  *%r15
	0x0000000100031499 <-[SSDocumentController checkLicense:]+212>:	lea    0x41c20(%rip),%rdx        # 0x1000730c0
	0x00000001000314a0 <-[SSDocumentController checkLicense:]+219>:	mov    0x3d239(%rip),%rsi        # 0x10006e6e0
	0x00000001000314a7 <-[SSDocumentController checkLicense:]+226>:	mov    %rbx,%rdi
	0x00000001000314aa <-[SSDocumentController checkLicense:]+229>:	callq  *%r15
	0x00000001000314ad <-[SSDocumentController checkLicense:]+232>:	lea    0x41c2c(%rip),%rdx        # 0x1000730e0
	0x00000001000314b4 <-[SSDocumentController checkLicense:]+239>:	mov    0x3d225(%rip),%rsi        # 0x10006e6e0
	0x00000001000314bb <-[SSDocumentController checkLicense:]+246>:	mov    %rbx,%rdi
	0x00000001000314be <-[SSDocumentController checkLicense:]+249>:	callq  *%r15
	0x00000001000314c1 <-[SSDocumentController checkLicense:]+252>:	lea    0x41c38(%rip),%r14        # 0x100073100
	0x00000001000314c8 <-[SSDocumentController checkLicense:]+259>:	mov    0x3d211(%rip),%rsi        # 0x10006e6e0
	0x00000001000314cf <-[SSDocumentController checkLicense:]+266>:	mov    %rbx,%rdi
	0x00000001000314d2 <-[SSDocumentController checkLicense:]+269>:	mov    %r14,%rdx
	0x00000001000314d5 <-[SSDocumentController checkLicense:]+272>:	callq  *%r15
	0x00000001000314d8 <-[SSDocumentController checkLicense:]+275>:	mov    0x3d201(%rip),%rsi        # 0x10006e6e0
	0x00000001000314df <-[SSDocumentController checkLicense:]+282>:	mov    %rbx,%rdi
	0x00000001000314e2 <-[SSDocumentController checkLicense:]+285>:	mov    %r14,%rdx
	0x00000001000314e5 <-[SSDocumentController checkLicense:]+288>:	callq  *%r15
	0x00000001000314e8 <-[SSDocumentController checkLicense:]+291>:	lea    0x41c31(%rip),%rdx        # 0x100073120
	0x00000001000314ef <-[SSDocumentController checkLicense:]+298>:	mov    0x3d1ea(%rip),%rsi        # 0x10006e6e0
	0x00000001000314f6 <-[SSDocumentController checkLicense:]+305>:	mov    %rbx,%rdi
	0x00000001000314f9 <-[SSDocumentController checkLicense:]+308>:	callq  *%r15
	0x00000001000314fc <-[SSDocumentController checkLicense:]+311>:	lea    0x41c3d(%rip),%rdx        # 0x100073140
	0x0000000100031503 <-[SSDocumentController checkLicense:]+318>:	mov    0x3d1d6(%rip),%rsi        # 0x10006e6e0
	0x000000010003150a <-[SSDocumentController checkLicense:]+325>:	mov    %rbx,%rdi
	0x000000010003150d <-[SSDocumentController checkLicense:]+328>:	callq  *%r15
	0x0000000100031510 <-[SSDocumentController checkLicense:]+331>:	mov    0x3d1c9(%rip),%rsi        # 0x10006e6e0
	0x0000000100031517 <-[SSDocumentController checkLicense:]+338>:	mov    %rbx,%rdi
	0x000000010003151a <-[SSDocumentController checkLicense:]+341>:	mov    %r13,%rdx
	0x000000010003151d <-[SSDocumentController checkLicense:]+344>:	callq  *%r15
	0x0000000100031520 <-[SSDocumentController checkLicense:]+347>:	mov    0x3d1b9(%rip),%rsi        # 0x10006e6e0
	0x0000000100031527 <-[SSDocumentController checkLicense:]+354>:	mov    %rbx,%rdi
	0x000000010003152a <-[SSDocumentController checkLicense:]+357>:	mov    %r13,%rdx
	0x000000010003152d <-[SSDocumentController checkLicense:]+360>:	callq  *%r15
	0x0000000100031530 <-[SSDocumentController checkLicense:]+363>:	lea    0x41c29(%rip),%rdx        # 0x100073160
	0x0000000100031537 <-[SSDocumentController checkLicense:]+370>:	mov    0x3d1a2(%rip),%rsi        # 0x10006e6e0
	0x000000010003153e <-[SSDocumentController checkLicense:]+377>:	mov    %rbx,%rdi
	0x0000000100031541 <-[SSDocumentController checkLicense:]+380>:	callq  *%r15
	0x0000000100031544 <-[SSDocumentController checkLicense:]+383>:	lea    0x41c35(%rip),%rdx        # 0x100073180
	0x000000010003154b <-[SSDocumentController checkLicense:]+390>:	mov    0x3d18e(%rip),%rsi        # 0x10006e6e0
	0x0000000100031552 <-[SSDocumentController checkLicense:]+397>:	mov    %rbx,%rdi
	0x0000000100031555 <-[SSDocumentController checkLicense:]+400>:	callq  *%r15
	0x0000000100031558 <-[SSDocumentController checkLicense:]+403>:	lea    0x41c41(%rip),%r13        # 0x1000731a0
	0x000000010003155f <-[SSDocumentController checkLicense:]+410>:	mov    0x3d17a(%rip),%rsi        # 0x10006e6e0
	0x0000000100031566 <-[SSDocumentController checkLicense:]+417>:	mov    %rbx,%rdi
	0x0000000100031569 <-[SSDocumentController checkLicense:]+420>:	mov    %r13,%rdx
	0x000000010003156c <-[SSDocumentController checkLicense:]+423>:	callq  *%r15
	0x000000010003156f <-[SSDocumentController checkLicense:]+426>:	mov    0x3d16a(%rip),%rsi        # 0x10006e6e0
	0x0000000100031576 <-[SSDocumentController checkLicense:]+433>:	mov    %rbx,%rdi
	0x0000000100031579 <-[SSDocumentController checkLicense:]+436>:	mov    %r13,%rdx
	0x000000010003157c <-[SSDocumentController checkLicense:]+439>:	callq  *%r15
	0x000000010003157f <-[SSDocumentController checkLicense:]+442>:	lea    0x41c3a(%rip),%rdx        # 0x1000731c0
	0x0000000100031586 <-[SSDocumentController checkLicense:]+449>:	mov    0x3d153(%rip),%rsi        # 0x10006e6e0
	0x000000010003158d <-[SSDocumentController checkLicense:]+456>:	mov    %rbx,%rdi
	0x0000000100031590 <-[SSDocumentController checkLicense:]+459>:	callq  *%r15
	0x0000000100031593 <-[SSDocumentController checkLicense:]+462>:	lea    0x41c46(%rip),%rdx        # 0x1000731e0
	0x000000010003159a <-[SSDocumentController checkLicense:]+469>:	mov    0x3d13f(%rip),%rsi        # 0x10006e6e0
	0x00000001000315a1 <-[SSDocumentController checkLicense:]+476>:	mov    %rbx,%rdi
	0x00000001000315a4 <-[SSDocumentController checkLicense:]+479>:	callq  *%r15
	0x00000001000315a7 <-[SSDocumentController checkLicense:]+482>:	mov    0x3d132(%rip),%rsi        # 0x10006e6e0
	0x00000001000315ae <-[SSDocumentController checkLicense:]+489>:	mov    %rbx,%rdi
	0x00000001000315b1 <-[SSDocumentController checkLicense:]+492>:	mov    %r12,%rdx
	0x00000001000315b4 <-[SSDocumentController checkLicense:]+495>:	callq  *%r15
	0x00000001000315b7 <-[SSDocumentController checkLicense:]+498>:	mov    0x3d122(%rip),%rsi        # 0x10006e6e0
	0x00000001000315be <-[SSDocumentController checkLicense:]+505>:	mov    %rbx,%rdi
	0x00000001000315c1 <-[SSDocumentController checkLicense:]+508>:	mov    %r12,%rdx
	0x00000001000315c4 <-[SSDocumentController checkLicense:]+511>:	callq  *%r15
	0x00000001000315c7 <-[SSDocumentController checkLicense:]+514>:	lea    0x41c32(%rip),%rdx        # 0x100073200
	0x00000001000315ce <-[SSDocumentController checkLicense:]+521>:	mov    0x3d10b(%rip),%rsi        # 0x10006e6e0
	0x00000001000315d5 <-[SSDocumentController checkLicense:]+528>:	mov    %rbx,%rdi
	0x00000001000315d8 <-[SSDocumentController checkLicense:]+531>:	callq  *%r15
	0x00000001000315db <-[SSDocumentController checkLicense:]+534>:	lea    0x41c3e(%rip),%rdx        # 0x100073220
	0x00000001000315e2 <-[SSDocumentController checkLicense:]+541>:	mov    0x3d0f7(%rip),%rsi        # 0x10006e6e0
	0x00000001000315e9 <-[SSDocumentController checkLicense:]+548>:	mov    %rbx,%rdi
	0x00000001000315ec <-[SSDocumentController checkLicense:]+551>:	callq  *%r15
	0x00000001000315ef <-[SSDocumentController checkLicense:]+554>:	lea    0x41c4a(%rip),%rdx        # 0x100073240
	0x00000001000315f6 <-[SSDocumentController checkLicense:]+561>:	mov    0x3d0e3(%rip),%rsi        # 0x10006e6e0
	0x00000001000315fd <-[SSDocumentController checkLicense:]+568>:	mov    %rbx,%rdi
	0x0000000100031600 <-[SSDocumentController checkLicense:]+571>:	callq  *%r15
	0x0000000100031603 <-[SSDocumentController checkLicense:]+574>:	mov    0x3d0d6(%rip),%rsi        # 0x10006e6e0
	0x000000010003160a <-[SSDocumentController checkLicense:]+581>:	mov    %rbx,%rdi
	0x000000010003160d <-[SSDocumentController checkLicense:]+584>:	mov    %r13,%rdx
	0x0000000100031610 <-[SSDocumentController checkLicense:]+587>:	callq  *%r15
	0x0000000100031613 <-[SSDocumentController checkLicense:]+590>:	mov    0x3d0c6(%rip),%rsi        # 0x10006e6e0
	0x000000010003161a <-[SSDocumentController checkLicense:]+597>:	mov    %rbx,%rdi
	0x000000010003161d <-[SSDocumentController checkLicense:]+600>:	mov    %r13,%rdx
	0x0000000100031620 <-[SSDocumentController checkLicense:]+603>:	callq  *%r15
	0x0000000100031623 <-[SSDocumentController checkLicense:]+606>:	lea    0x41c36(%rip),%rdx        # 0x100073260
	0x000000010003162a <-[SSDocumentController checkLicense:]+613>:	mov    0x3d0af(%rip),%rsi        # 0x10006e6e0
	0x0000000100031631 <-[SSDocumentController checkLicense:]+620>:	mov    %rbx,%rdi
	0x0000000100031634 <-[SSDocumentController checkLicense:]+623>:	callq  *%r15
	0x0000000100031637 <-[SSDocumentController checkLicense:]+626>:	lea    0x41c42(%rip),%rdx        # 0x100073280
	0x000000010003163e <-[SSDocumentController checkLicense:]+633>:	mov    0x3d09b(%rip),%rsi        # 0x10006e6e0
	0x0000000100031645 <-[SSDocumentController checkLicense:]+640>:	mov    %rbx,%rdi
	0x0000000100031648 <-[SSDocumentController checkLicense:]+643>:	callq  *%r15
	0x000000010003164b <-[SSDocumentController checkLicense:]+646>:	mov    0x3deae(%rip),%rsi        # 0x10006f500
	0x0000000100031652 <-[SSDocumentController checkLicense:]+653>:	mov    0x3e827(%rip),%rdi        # 0x10006fe80
	0x0000000100031659 <-[SSDocumentController checkLicense:]+660>:	mov    %rbx,%rdx
	0x000000010003165c <-[SSDocumentController checkLicense:]+663>:	callq  *%r15
	0x000000010003165f <-[SSDocumentController checkLicense:]+666>:	mov    %rax,%rdi
	0x0000000100031662 <-[SSDocumentController checkLicense:]+669>:	callq  0x100040ef6 <dyld_stub_objc_retainAutoreleasedReturnValue>
	0x0000000100031667 <-[SSDocumentController checkLicense:]+674>:	mov    %rax,%r12
	0x000000010003166a <-[SSDocumentController checkLicense:]+677>:	mov    0x3de97(%rip),%rsi        # 0x10006f508
	0x0000000100031671 <-[SSDocumentController checkLicense:]+684>:	mov    %r12,%rdi
	0x0000000100031674 <-[SSDocumentController checkLicense:]+687>:	mov    -0x38(%rbp),%r13
	0x0000000100031678 <-[SSDocumentController checkLicense:]+691>:	mov    %r13,%rdx
	0x000000010003167b <-[SSDocumentController checkLicense:]+694>:	callq  *%r15
	0x000000010003167e <-[SSDocumentController checkLicense:]+697>:	mov    %rax,%r14
	0x0000000100031681 <-[SSDocumentController checkLicense:]+700>:	mov    %r13,%rdi
	0x0000000100031684 <-[SSDocumentController checkLicense:]+703>:	callq  0x100040ede <dyld_stub_objc_release>
	0x0000000100031689 <-[SSDocumentController checkLicense:]+708>:	mov    %r14,%rdi
	0x000000010003168c <-[SSDocumentController checkLicense:]+711>:	callq  0x100040ef6 <dyld_stub_objc_retainAutoreleasedReturnValue>
	0x0000000100031691 <-[SSDocumentController checkLicense:]+716>:	mov    %rax,%r14
	0x0000000100031694 <-[SSDocumentController checkLicense:]+719>:	mov    0x3dd6d(%rip),%rsi        # 0x10006f408
	0x000000010003169b <-[SSDocumentController checkLicense:]+726>:	mov    -0x30(%rbp),%r13
	0x000000010003169f <-[SSDocumentController checkLicense:]+730>:	mov    %r13,%rdi
	0x00000001000316a2 <-[SSDocumentController checkLicense:]+733>:	mov    %r14,%rdx
	0x00000001000316a5 <-[SSDocumentController checkLicense:]+736>:	callq  *%r15
	0x00000001000316a8 <-[SSDocumentController checkLicense:]+739>:	mov    %r14,%rdi
	0x00000001000316ab <-[SSDocumentController checkLicense:]+742>:	callq  0x100040ede <dyld_stub_objc_release>
	0x00000001000316b0 <-[SSDocumentController checkLicense:]+747>:	mov    0x3de59(%rip),%rsi        # 0x10006f510
	0x00000001000316b7 <-[SSDocumentController checkLicense:]+754>:	mov    %r12,%rdi
	0x00000001000316ba <-[SSDocumentController checkLicense:]+757>:	callq  *%r15
	0x00000001000316bd <-[SSDocumentController checkLicense:]+760>:	mov    %rax,%rdi
	0x00000001000316c0 <-[SSDocumentController checkLicense:]+763>:	callq  0x100040ef6 <dyld_stub_objc_retainAutoreleasedReturnValue>
	0x00000001000316c5 <-[SSDocumentController checkLicense:]+768>:	mov    %rax,%r14
	0x00000001000316c8 <-[SSDocumentController checkLicense:]+771>:	mov    0x3de49(%rip),%rsi        # 0x10006f518
	0x00000001000316cf <-[SSDocumentController checkLicense:]+778>:	mov    %r13,%rdi
	0x00000001000316d2 <-[SSDocumentController checkLicense:]+781>:	mov    %r14,%rdx
	0x00000001000316d5 <-[SSDocumentController checkLicense:]+784>:	callq  *%r15
	0x00000001000316d8 <-[SSDocumentController checkLicense:]+787>:	mov    %r14,%rdi
	0x00000001000316db <-[SSDocumentController checkLicense:]+790>:	callq  0x100040ede <dyld_stub_objc_release>
	0x00000001000316e0 <-[SSDocumentController checkLicense:]+795>:	mov    %r12,%rdi
	0x00000001000316e3 <-[SSDocumentController checkLicense:]+798>:	callq  0x100040ede <dyld_stub_objc_release>
	0x00000001000316e8 <-[SSDocumentController checkLicense:]+803>:	mov    %rbx,%rdi
	0x00000001000316eb <-[SSDocumentController checkLicense:]+806>:	add    $0x18,%rsp
	0x00000001000316ef <-[SSDocumentController checkLicense:]+810>:	pop    %rbx
	0x00000001000316f0 <-[SSDocumentController checkLicense:]+811>:	pop    %r12
	0x00000001000316f2 <-[SSDocumentController checkLicense:]+813>:	pop    %r13
	0x00000001000316f4 <-[SSDocumentController checkLicense:]+815>:	pop    %r14
	0x00000001000316f6 <-[SSDocumentController checkLicense:]+817>:	pop    %r15
	0x00000001000316f8 <-[SSDocumentController checkLicense:]+819>:	pop    %rbp
	0x00000001000316f9 <-[SSDocumentController checkLicense:]+820>:	jmpq   0x100040ede <dyld_stub_objc_release>
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


