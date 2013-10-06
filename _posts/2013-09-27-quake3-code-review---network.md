---
layout: post
title: "Quake3 code review Network"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Client

	CL_SendCmd
	
	CL_WritePacket
	
	CL_Netchan_Transmit
	
	Netchan_Transmit
	
	NET_SendPacket // 装包, 添加包头等
	
	Sys_SendPacket // 调用系统 socket, 发送
	
#Server
###怎么分发所有玩家的 game state?

	SV_SendQueuedMessages()
	{
		for(i=0; i < clients.count; i++)
		{
			SV_Netchan_TransmitNextFragment()
			{
				SV_Netchan_TransmitNextInQueue()
				{
					Netchan_Transmit()
					{
						NET_SendPacket()
						{
							// 如果只有一个玩家,直接发送
							Sys_SendPacket()
							// 如果多个玩家,把所有 clients.data 塞到一个队列里
							NET_QueuePacket()
							{
								
							}
						}
					}
				}
			}
		}
		
	}
	
	
common.c 中调用:

	NET_FlushPacketQueue()
	{
		// 传送队列中的所有 data
		while(packetQueue) {
			Sys_SendPacket(packetQueue->length, packetQueue->data,
				packetQueue->to);
			last = packetQueue;
			packetQueue = packetQueue->next;
			Z_Free(last->data);
			Z_Free(last);
		}
	}
	
###客户端怎么解析所有玩家的信息?

	CL_ParseServerMessage
	
	
	
	

