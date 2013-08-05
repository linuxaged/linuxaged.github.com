---
layout: post
title: "IList vs List vs ArrayList in C#"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Reference

[link 1](http://www.cnblogs.com/Godblessyou/archive/2011/05/05/2037572.html)

#Differences

	IList and List can only store same type values, but ArrayList can store diff types, For example:


	IList an_ilist = new IList<string>();
	an_ilist.Add("this")
	an_ilist.Add("is")
	an_ilist.Add("ilist")

	List a_list = new List<string>();

	ArrayList an_arraylist = new ArrayList();
	an_arrayList.Add("Hello");
	an_arraylist.Add(99);

#Performence

	When the size of data need to store larger than 50000, List<T> performs better. 
	So in common use ArrayList as far as possible