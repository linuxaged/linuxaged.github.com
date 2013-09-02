---
layout: post
title: "Linode CentOS VPS as VPN Server"
description: ""
category: 
tags: []
---
{% include JB/setup %}

	yum install ppp
	// yum 包里找不到 pptpd, 手动下载安装
	wget http://poptop.sourceforge.net/yum/stable/packages/pptpd-1.3.4-2.el6.x86_64.rpm
	rpm -Uhv pptpd-1.3.4-2.el6.x86_64.rpm