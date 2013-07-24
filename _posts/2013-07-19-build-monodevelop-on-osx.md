---
layout: post
title: "Build MonoDevelop On OSX"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Install Mono
Download the Mono, install.

Then set the PATH

	export PATH=$PATH:/Library/Frameworks/Mono.framework/Versions/2.10.11/bin
	
	
#Build

	make
	cd main/build/MacOSX
	make all