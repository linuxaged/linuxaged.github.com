---
layout: post
title: "How to backup others blog"
description: ""
category: 
tags: []
---
{% include JB/setup %}

快放长假了, 要在家呆几天, 但没有宽带, 总要找些事做做的.
所以准备备份几个博客回去读读, 像 [云风](http://blog.codingnow.com/) , [JULY](http://blog.csdn.net/v_JULY_v) 的都很有营养.


利用工具 [HTTrack](http://www.httrack.com/), 这个项目维护的很好, 提供各个版本.

支持 Mac 下 brew 安装

	brew install httrack
	
还提供过滤, 如果不想要一些图片的话:
	
	You can define wildcards, like: -*.gif +www.*.com/*.zip -*img_*.zip
	Wildcards (return=none) :
	
试试看吧!

---
csdn 的 blog 用上面这个工具 down 不了. 原来 wget 就可抓取整个网站:

	wget -r -p -U Mozilla http://blog.csdn.net/v_JULY_v