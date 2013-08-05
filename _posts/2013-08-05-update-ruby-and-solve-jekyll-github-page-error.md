---
layout: post
title: "Update ruby and solve jekyll github page error"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Update ruby

	curl -L https://get.rvm.io | bash -s stable --ruby


#Install new version jekyll

	gem install jekyll
	
#Fix errors after build

	jekyll build