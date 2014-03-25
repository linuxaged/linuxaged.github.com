---
layout: post
title: "Sublime Text Install GoSublime"
description: ""
category: 
tags: []
---
{% include JB/setup %}

即使在 ~/.bashrc 里设置好 **GOPATH** **GOROOT** 仍然会报错:

	MarGo: Missing required environment variables: GOPATH
	See the `Quirks` section of USAGE.md for info
	
解决方法:

Preferences -> Package Settings -> GoSublime -> Settings -> User

添加:

	{"env": {"GOPATH": "$HOME/go/", "GOROOT": "/usr/local/go"}}