---
layout: post
title: "C++11 Memory Model"
description: ""
category: 
tags: []
---
{% include JB/setup %}


分为

#structural aspects

#concurrency aspects

如果多个线程操作同一块内存,他们必须按照事先定好的次序来

用原子操作保证次序


##原子操作&原子类型