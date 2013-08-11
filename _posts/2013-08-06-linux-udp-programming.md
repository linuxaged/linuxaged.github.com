---
layout: post
title: "Linux UDP Programming"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#IP,UDP
IP 层负责路由 以及 拆包,装包 

#Socket Basics

	sockaddr_in
	操作  bind, connect, sendto, sendmsg
	inet_aton, inet_ntoa, and inet_addr

#Quake3 Network Model

	Client    --commands-->   Server
             <--gamestate--   
             
             
#最简 UDP Server      

#并发