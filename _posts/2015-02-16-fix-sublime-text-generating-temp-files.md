---
layout: post
title: "Fix Sublime Text Generating Temp Files"
description: ""
category: 
tags: [sublime text]
---
{% include JB/setup %}

从 3065 版本升级到 3069 版本遇到一个问题：从命令行调用 Sublime Text 3 编辑文本保存的时候，会在当前目录下创建若干个 tempxxxxx 文件

	➜  ls
		main      readme.md tmp05pop7 tmp2rezqm tmpeh0m5s tmplzdv4n tmpp060cx tmpq_r3rh 		tmpt627ra tmpv6597_ tmpwr0kr2
		main.rs   src       tmp2qw2fm tmp6zxjq8 tmpkns46m tmpo1fn1o tmpp1a6gd tmprjrw51 		tmpuap19f tmpw88tln
		
原以为是指向命令行的软链接引起的，重新创建指向命令行的软链接：
	
	➜ rm -f /usr/local/bin/subl

	➜ ln -s "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" /usr/local/bin/subl
	
发现并没有解决问题。

我又猜测是 Rust 相关的插件引起的，因为今天更新了 rustc 版本，可能 rust autocomplete 这个插件还没有更新到最新。去掉了 Rust Autocomplete 插件，到目前为止这个问题还没重现。
	
