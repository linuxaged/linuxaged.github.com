---
layout: post
title: "how does computer represent decimal"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#为什么计算机没法精确的表示小数?
这就和十进制没法准确的表示分数一样,比如 1/3, 用小数表示就是 0.3333333...(无限循环)

十进制表示的基底(base)是 10,也就是说任何数在十进制系统内都可以表示成 10^n 的若干组合


	0.33333333 = 0.3 + 0.03 + 0.003 + 0.0003 … = 3 * 10^-1 + 3 * 10^-2 + 3 * 10^-3 …

如果计算机是采用的十进制,他也没法精确的用十进制表示这个 1/3,因为现实中我们没法造出拥有无穷个bit的机器.