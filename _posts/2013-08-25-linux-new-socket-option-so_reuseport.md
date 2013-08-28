---
layout: post
title: "Linux New Socket Option SO_REUSEPORT"
description: ""
category: 
tags: []
---
{% include JB/setup %}

开启 SO_REUSEPORT 选项, 就可以把 socket 绑定到同一地址的同一端口, 为了避免端口挟持所有 socket 所在进程必须共用同一个 UID

比如在一个 web server 中多个线程绑定到 80 端口