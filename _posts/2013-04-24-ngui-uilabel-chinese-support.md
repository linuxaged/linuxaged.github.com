---
layout: post
title: "NGUI UILabel Chinese Support"
description: ""
category: 
tags: []
---
{% include JB/setup %}

You have set the code format to UTF-8, but also get the "???"

`UILabel c = GameObject.Find("below_Label_company").GetComponent<UILabel>();
        c.text = "汉字";`
        
OK, save your code as "UTF-8 with BOM".
