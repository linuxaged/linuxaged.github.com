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
	[checkLicense:]+0>:	push   %rbp
	[checkLicense:]+1>:	mov    %rsp,%rbp
	[checkLicense:]+4>:	push   %r15
	[checkLicense:]+6>:	push   %r14
	[checkLicense:]+8>:	push   %r13
	[checkLicense:]+10>:	push   %r12
	[checkLicense:]+12>:	push   %rbx
	[checkLicense:]+13>:	sub    $0x18,%rsp
	[checkLicense:]+17>:	mov    %rdi,-0x30(%rbp)
	[checkLicense:]+21>:	mov    0x3e117(%rip),%r14        # 0x10006f4f8
	[checkLicense:]+28>:	mov    0x3e970(%rip),%rbx        # 0x10006fd58
	[checkLicense:]+35>:	mov    %rdx,%rdi
	[checkLicense:]+38>:	callq  0x100040ee4 <dyld_stub_objc_retain>
	[checkLicense:]+43>:	mov    %rax,-0x38(%rbp)
	[checkLicense:]+47>:	mov    0x2cd5d(%rip),%r15        # 0x10005e158
	[checkLicense:]+54>:	mov    %rbx,%rdi
	[checkLicense:]+57>:	mov    %r14,%rsi
	[checkLicense:]+60>:	callq  *%r15
	[checkLicense:]+63>:	mov    %rax,%rdi
	[checkLicense:]+66>:	callq  0x100040ef6 <dyld_stub_objc_retainAutoreleasedReturnValue>
	[checkLicense:]+71>:	mov    %rax,%rbx
	[checkLicense:]+74>:	lea    0x41c0a(%rip),%rdx        # 0x100073020
	[checkLicense:]+81>:	mov    0x3d2c3(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+88>:	mov    %rbx,%rdi
	[checkLicense:]+91>:	callq  *%r15
	[checkLicense:]+94>:	lea    0x41c16(%rip),%r13        # 0x100073040
	[checkLicense:]+101>:	mov    0x3d2af(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+108>:	mov    %rbx,%rdi
	[checkLicense:]+111>:	mov    %r13,%rdx
	[checkLicense:]+114>:	callq  *%r15
	[checkLicense:]+117>:	mov    0x3d29f(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+124>:	mov    %rbx,%rdi
	[checkLicense:]+127>:	mov    %r13,%rdx
	[checkLicense:]+130>:	callq  *%r15
	[checkLicense:]+133>:	lea    0x41c0f(%rip),%rdx        # 0x100073060
	[checkLicense:]+140>:	mov    0x3d288(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+147>:	mov    %rbx,%rdi
	[checkLicense:]+150>:	callq  *%r15
	[checkLicense:]+153>:	lea    0x41c1b(%rip),%rdx        # 0x100073080
	[checkLicense:]+160>:	mov    0x3d274(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+167>:	mov    %rbx,%rdi
	[checkLicense:]+170>:	callq  *%r15
	[checkLicense:]+173>:	lea    0x41c27(%rip),%r12        # 0x1000730a0
	[checkLicense:]+180>:	mov    0x3d260(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+187>:	mov    %rbx,%rdi
	[checkLicense:]+190>:	mov    %r12,%rdx
	[checkLicense:]+193>:	callq  *%r15
	[checkLicense:]+196>:	mov    0x3d250(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+203>:	mov    %rbx,%rdi
	[checkLicense:]+206>:	mov    %r12,%rdx
	[checkLicense:]+209>:	callq  *%r15
	[checkLicense:]+212>:	lea    0x41c20(%rip),%rdx        # 0x1000730c0
	[checkLicense:]+219>:	mov    0x3d239(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+226>:	mov    %rbx,%rdi
	[checkLicense:]+229>:	callq  *%r15
	[checkLicense:]+232>:	lea    0x41c2c(%rip),%rdx        # 0x1000730e0
	[checkLicense:]+239>:	mov    0x3d225(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+246>:	mov    %rbx,%rdi
	[checkLicense:]+249>:	callq  *%r15
	[checkLicense:]+252>:	lea    0x41c38(%rip),%r14        # 0x100073100
	[checkLicense:]+259>:	mov    0x3d211(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+266>:	mov    %rbx,%rdi
	[checkLicense:]+269>:	mov    %r14,%rdx
	[checkLicense:]+272>:	callq  *%r15
	[checkLicense:]+275>:	mov    0x3d201(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+282>:	mov    %rbx,%rdi
	[checkLicense:]+285>:	mov    %r14,%rdx
	[checkLicense:]+288>:	callq  *%r15
	[checkLicense:]+291>:	lea    0x41c31(%rip),%rdx        # 0x100073120
	[checkLicense:]+298>:	mov    0x3d1ea(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+305>:	mov    %rbx,%rdi
	[checkLicense:]+308>:	callq  *%r15
	[checkLicense:]+311>:	lea    0x41c3d(%rip),%rdx        # 0x100073140
	[checkLicense:]+318>:	mov    0x3d1d6(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+325>:	mov    %rbx,%rdi
	[checkLicense:]+328>:	callq  *%r15
	[checkLicense:]+331>:	mov    0x3d1c9(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+338>:	mov    %rbx,%rdi
	[checkLicense:]+341>:	mov    %r13,%rdx
	[checkLicense:]+344>:	callq  *%r15
	[checkLicense:]+347>:	mov    0x3d1b9(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+354>:	mov    %rbx,%rdi
	[checkLicense:]+357>:	mov    %r13,%rdx
	[checkLicense:]+360>:	callq  *%r15
	[checkLicense:]+363>:	lea    0x41c29(%rip),%rdx        # 0x100073160
	[checkLicense:]+370>:	mov    0x3d1a2(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+377>:	mov    %rbx,%rdi
	[checkLicense:]+380>:	callq  *%r15
	[checkLicense:]+383>:	lea    0x41c35(%rip),%rdx        # 0x100073180
	[checkLicense:]+390>:	mov    0x3d18e(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+397>:	mov    %rbx,%rdi
	[checkLicense:]+400>:	callq  *%r15
	[checkLicense:]+403>:	lea    0x41c41(%rip),%r13        # 0x1000731a0
	[checkLicense:]+410>:	mov    0x3d17a(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+417>:	mov    %rbx,%rdi
	[checkLicense:]+420>:	mov    %r13,%rdx
	[checkLicense:]+423>:	callq  *%r15
	[checkLicense:]+426>:	mov    0x3d16a(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+433>:	mov    %rbx,%rdi
	[checkLicense:]+436>:	mov    %r13,%rdx
	[checkLicense:]+439>:	callq  *%r15
	[checkLicense:]+442>:	lea    0x41c3a(%rip),%rdx        # 0x1000731c0
	[checkLicense:]+449>:	mov    0x3d153(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+456>:	mov    %rbx,%rdi
	[checkLicense:]+459>:	callq  *%r15
	[checkLicense:]+462>:	lea    0x41c46(%rip),%rdx        # 0x1000731e0
	[checkLicense:]+469>:	mov    0x3d13f(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+476>:	mov    %rbx,%rdi
	[checkLicense:]+479>:	callq  *%r15
	[checkLicense:]+482>:	mov    0x3d132(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+489>:	mov    %rbx,%rdi
	[checkLicense:]+492>:	mov    %r12,%rdx
	[checkLicense:]+495>:	callq  *%r15
	[checkLicense:]+498>:	mov    0x3d122(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+505>:	mov    %rbx,%rdi
	[checkLicense:]+508>:	mov    %r12,%rdx
	[checkLicense:]+511>:	callq  *%r15
	[checkLicense:]+514>:	lea    0x41c32(%rip),%rdx        # 0x100073200
	[checkLicense:]+521>:	mov    0x3d10b(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+528>:	mov    %rbx,%rdi
	[checkLicense:]+531>:	callq  *%r15
	[checkLicense:]+534>:	lea    0x41c3e(%rip),%rdx        # 0x100073220
	[checkLicense:]+541>:	mov    0x3d0f7(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+548>:	mov    %rbx,%rdi
	[checkLicense:]+551>:	callq  *%r15
	[checkLicense:]+554>:	lea    0x41c4a(%rip),%rdx        # 0x100073240
	[checkLicense:]+561>:	mov    0x3d0e3(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+568>:	mov    %rbx,%rdi
	[checkLicense:]+571>:	callq  *%r15
	[checkLicense:]+574>:	mov    0x3d0d6(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+581>:	mov    %rbx,%rdi
	[checkLicense:]+584>:	mov    %r13,%rdx
	[checkLicense:]+587>:	callq  *%r15
	[checkLicense:]+590>:	mov    0x3d0c6(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+597>:	mov    %rbx,%rdi
	[checkLicense:]+600>:	mov    %r13,%rdx
	[checkLicense:]+603>:	callq  *%r15
	[checkLicense:]+606>:	lea    0x41c36(%rip),%rdx        # 0x100073260
	[checkLicense:]+613>:	mov    0x3d0af(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+620>:	mov    %rbx,%rdi
	[checkLicense:]+623>:	callq  *%r15
	[checkLicense:]+626>:	lea    0x41c42(%rip),%rdx        # 0x100073280
	[checkLicense:]+633>:	mov    0x3d09b(%rip),%rsi        # 0x10006e6e0
	[checkLicense:]+640>:	mov    %rbx,%rdi
	[checkLicense:]+643>:	callq  *%r15
	[checkLicense:]+646>:	mov    0x3deae(%rip),%rsi        # 0x10006f500
	[checkLicense:]+653>:	mov    0x3e827(%rip),%rdi        # 0x10006fe80
	[checkLicense:]+660>:	mov    %rbx,%rdx
	[checkLicense:]+663>:	callq  *%r15
	[checkLicense:]+666>:	mov    %rax,%rdi
	[checkLicense:]+669>:	callq  0x100040ef6 <dyld_stub_objc_retainAutoreleasedReturnValue>
	[checkLicense:]+674>:	mov    %rax,%r12
	[checkLicense:]+677>:	mov    0x3de97(%rip),%rsi        # 0x10006f508
	[checkLicense:]+684>:	mov    %r12,%rdi
	[checkLicense:]+687>:	mov    -0x38(%rbp),%r13
	[checkLicense:]+691>:	mov    %r13,%rdx
	[checkLicense:]+694>:	callq  *%r15
	[checkLicense:]+697>:	mov    %rax,%r14
	[checkLicense:]+700>:	mov    %r13,%rdi
	[checkLicense:]+703>:	callq  0x100040ede <dyld_stub_objc_release>
	[checkLicense:]+708>:	mov    %r14,%rdi
	[checkLicense:]+711>:	callq  0x100040ef6 <dyld_stub_objc_retainAutoreleasedReturnValue>
	[checkLicense:]+716>:	mov    %rax,%r14
	[checkLicense:]+719>:	mov    0x3dd6d(%rip),%rsi        # 0x10006f408
	[checkLicense:]+726>:	mov    -0x30(%rbp),%r13
	[checkLicense:]+730>:	mov    %r13,%rdi
	[checkLicense:]+733>:	mov    %r14,%rdx
	[checkLicense:]+736>:	callq  *%r15
	[checkLicense:]+739>:	mov    %r14,%rdi
	[checkLicense:]+742>:	callq  0x100040ede <dyld_stub_objc_release>
	[checkLicense:]+747>:	mov    0x3de59(%rip),%rsi        # 0x10006f510
	[checkLicense:]+754>:	mov    %r12,%rdi
	[checkLicense:]+757>:	callq  *%r15
	[checkLicense:]+760>:	mov    %rax,%rdi
	[checkLicense:]+763>:	callq  0x100040ef6 <dyld_stub_objc_retainAutoreleasedReturnValue>
	[checkLicense:]+768>:	mov    %rax,%r14
	[checkLicense:]+771>:	mov    0x3de49(%rip),%rsi        # 0x10006f518
	[checkLicense:]+778>:	mov    %r13,%rdi
	[checkLicense:]+781>:	mov    %r14,%rdx
	[checkLicense:]+784>:	callq  *%r15
	[checkLicense:]+787>:	mov    %r14,%rdi
	[checkLicense:]+790>:	callq  0x100040ede <dyld_stub_objc_release>
	[checkLicense:]+795>:	mov    %r12,%rdi
	[checkLicense:]+798>:	callq  0x100040ede <dyld_stub_objc_release>
	[checkLicense:]+803>:	mov    %rbx,%rdi
	[checkLicense:]+806>:	add    $0x18,%rsp
	[checkLicense:]+810>:	pop    %rbx
	[checkLicense:]+811>:	pop    %r12
	[checkLicense:]+813>:	pop    %r13
	[checkLicense:]+815>:	pop    %r14
	[checkLicense:]+817>:	pop    %r15
	[checkLicense:]+819>:	pop    %rbp
	[checkLicense:]+820>:	jmpq   0x100040ede <dyld_stub_objc_release>
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


