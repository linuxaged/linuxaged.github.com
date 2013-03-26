---
layout: post
title: "Unity Network Overview"
description: ""
category: 
tags: []
---
{% include JB/setup %}
#分类
##Authoritative Server
这种模式要求服务器端处理场景模拟,游戏逻辑,用户输入…
客户端要做的仅仅是发出请求,接受结果,然后处理

优点:
减少作弊可能性.

缺点:
较长的延时.

解决方法:
允许客户端维护 local state,然后定期接受服务器端的更新

##Non-Authoritative Server
客户端处理用户输入和游戏逻辑,然后把所有的diff传送给服务器,服务器同步所有的 world state

  ---

#Methods of Network Communication
##Remote Procedure Calls

常被用来处理低频事件
##State Synchronization

用来处理实时事件