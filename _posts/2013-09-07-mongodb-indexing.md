---
layout: post
title: "MongoDB Indexing"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#[Basic](http://docs.mongodb.org/manual/indexes/)
创建一个索引

	// 为 user collection 创建一个 username 字段的索引
	db.users.ensureIndex({"name":1})
	// 创建 compound index
	db.users.ensureIndex({"name":1, "email":1})
	
	
index 也是需要代价的, 每次 write collection 的时候会更新 index.
MongoDB 限制每个 collection 最多 64 index.


#[利用索引排序查询结果](http://docs.mongodb.org/manual/tutorial/sort-results-with-indexes/)

	db.users.find().sort({name:1})