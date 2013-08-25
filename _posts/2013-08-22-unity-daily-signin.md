---
layout: post
title: "Unity Daily Signin"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#每日签到

	void Start () {
		lastDay = DateTime.Now.Day;
	}
	…
	if (DateTime.Now.Day - lastDay == 1) {
	    ++signinDays;
	}