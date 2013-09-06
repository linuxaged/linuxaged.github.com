---
layout: post
title: "Install MongoDB on Debian 7"
description: ""
category: 
tags: []
---
{% include JB/setup %}

[Install mongodb On Debian](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-debian/)

[Build mongodb on linux](http://www.mongodb.org/about/tutorial/build-mongodb-on-linux/)

[Create a User Administrator](http://docs.mongodb.org/manual/tutorial/add-user-administrator/)

#Install from source

#Authentication
	/etc/init.d/mongodb stop
	
	mongod --auth --fork --logpath=/var/log/mongodb/mongodb.log --setParameter enableLocalhostAuthBypass=0