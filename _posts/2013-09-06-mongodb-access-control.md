---
layout: post
title: "MongoDB Access Control"
description: ""
category: 
tags: []
---
{% include JB/setup %}

[MongoDB Access Control](http://docs.mongodb.org/manual/core/access-control/)

#先创建根用户
可以通过两种方式创建根用户,

	1.已经存在根用户
	2.开启 enableLocalhostAuthBypass=1 选项
	
#[Add User to a db](http://docs.mongodb.org/manual/tutorial/add-user-to-database/)

	> use test
	> db.addUser({user:"Alice",pwd:"Moon1234",roles:[ "readWrite","dbAdmin"]})