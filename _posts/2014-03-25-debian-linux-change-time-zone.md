---
layout: post
title: "Debian Linux Change Time Zone"
description: ""
category: 
tags: []
---
{% include JB/setup %}

	// 比如要使用上海时间
	echo "Asia/Shanghai" | sudo tee /etc/timezone
	sudo dpkg-reconfigure --frontend noninteractive tzdata