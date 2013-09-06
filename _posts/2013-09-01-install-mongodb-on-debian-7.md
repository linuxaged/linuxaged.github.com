---
layout: post
title: "Install MongoDB on Debian 7"
description: ""
category: 
tags: []
---
{% include JB/setup %}

[Install mongodb On Debian](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-debian/)

[Create a User Administrator](http://docs.mongodb.org/manual/tutorial/add-user-administrator/)

#Authentication
	/etc/init.d/mongodb stop
	
	mongod --auth --fork --logpath=/var/log/mongodb/mongodb.log