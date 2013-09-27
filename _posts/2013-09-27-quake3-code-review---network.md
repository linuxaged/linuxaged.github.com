---
layout: post
title: "Quake3 code review   Network"
description: ""
category: 
tags: []
---
{% include JB/setup %}

	CL_WritePacket
	
	CL_Netchan_Transmit
	
	Netchan_Transmit
	
	NET_SendPacket // 装包, 添加包头等
	
	Sys_SendPacket // 调用系统 socket, 发送
