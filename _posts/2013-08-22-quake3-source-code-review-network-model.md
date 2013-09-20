---
layout: post
title: "Quake3 Source Code Review: Network Model"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#翻译自
[QUAKE3 SOURCE CODE REVIEW : NETWORK MODEL](http://fabiensanglard.net/quake3/network.php)

#背景
Quake3 的网络模型无疑是整个 **idTech3** 引擎最优雅的部分.它沿用了最先在 **Quake World** 中使用的 [NetChannel](http://fabiensanglard.net/quakeSource/quakeSourceNetWork.php) 模型.

要明白这个模型, 最重要的是理解: 凡是无法在正常延时内到达的数据都要丢掉.

因此 Quake3 选择了 UDP 协议. TCP这种有保障的传输存在的延迟是无法接受的.

#实现
这个模型在经典的网络栈上又套了一层: 加密(preshared key) 和 压缩(huffman)

Quake3 Network 的点睛之笔是在服务端利用 memory introspection 计算快照之间的偏差, 然后只发送这个偏差, 这样就减小了 UDP 报文的大小从而弥补了 UDP 不可靠的缺陷.

##Arch
客户端比较简单, 就是发送 commands 到服务器然后接受 gamestate 的更新,
服务器端稍复杂, 在广播 Master gamestate 的同时需要审查报文的丢失情况

服务器用一个环结构保存每个客户端 32 个历史 gamestate (称作 快照)

##代码
	adler32          be_ai_gen     con_tty    l_script     sv_game
	be_aas_bspq3     be_ai_goal    crc32      l_struct     sv_init
	be_aas_cluster   be_ai_move    cvar       md4          sv_main
	be_aas_debug     be_ai_weap    files      msg          sv_net_chan
	be_aas_entity    be_ai_weight  ftola      net_chan     sv_snapshot
	be_aas_file      be_ea         huffman    net_ip       sv_world
	be_aas_main      be_interface  inffast    null_client  sys_main
	be_aas_move      cmd           inflate    null_input   sys_unix
	be_aas_optimize  cm_load       inftrees   null_snddma  unzip
	be_aas_reach     cm_patch      ioapi      q_math       vm
												           vm_interpreted
	be_aas_route     cm_test       l_libvar   snapvector   vm_x86
	be_aas_sample    cm_trace      l_log      sv_bot       zutil
	be_ai_char       common        l_memory   sv_ccmds
	be_ai_chat       con_log       l_precomp  sv_client


___
PS

ioquake 在 OSX 下的编译
	
	$ git clone https://github.com/ioquake/ioq3.git
	$ cd ioq3
	$ make COMPILE_ARCH=x86_64 BUILD_SERVER=0
