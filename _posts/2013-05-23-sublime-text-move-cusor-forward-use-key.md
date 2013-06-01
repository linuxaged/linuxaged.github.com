---
layout: post
title: "Sublime Text Move Cusor forward Use Key"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Step 1

打开 Preferences -> Key Bindings User

#Step 2

添加这一行

`{ "keys": ["shift+space"], "command": "move", "args": {"by": "characters", "forward": true} }`