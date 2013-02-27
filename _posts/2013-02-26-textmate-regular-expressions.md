---
layout: post
title: "Textmate Regular Expressions"
description: ""
category: 
tags: []
---
{% include JB/setup %}
#参考
http://manual.macromates.com/en/regular_expressions#regular_expressions
http://deerchao.net/tutorials/regex/regex.htm#greedyandlazy
#入门
1.匹配电话号码
	0\d\d-\d\d\d\d\d\d\d\d
或者简写成 0\d{2}-\d{8}

2.匹配5到12位QQ号
	^\d{5,12}$
^和$表示整个字符串必须是5到12的数字

3.下面是一个更复杂的表达式：
	\(?0\d{2}[) -]?\d{8}
这个表达式可以匹配几种格式的电话号码，像(010)88886666，或022-22334455，或02912345678等。
我们对它进行一些分析吧：首先是一个转义字符\(,它能出现0次或1次(?),然后是一个0，后面跟着2个数字(\d{2})，
然后是)或-或空格中的一个，它出现1次或不出现(?)，最后是8个数字(\d{8})。

4.正则匹配中文汉字
正则匹配中文汉字根据页面编码不同而略有区别：
GBK/GB2312编码：[x80-xff>]+ 或 [xa1-xff]+
UTF-8编码：[x{4e00}-x{9fa5}]+/u

#后向匹配
1.匹配重复的单词
	\b(\w+)\b\s+\1\b	
2.匹配重复的汉字
第一步,汉字在unicode中的范围 [\x{4e00}-\x{9fa5}]
