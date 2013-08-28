---
layout: post
title: "Unity XCode iOS Project addSubView"
description: ""
category: 
tags: []
---
{% include JB/setup %}

经常会有添加广告的需求, 在 unity view 上面加一个 subview

###untiy version 3.x
	// 找到 OpenEAGL_UnityCallback() 函数, 
	// 在
	// sGLViewController = controller;
    // sGLView = view;
    // 后面添加
    BannerViewController* bannerViewController = [[BannerViewController alloc]init];
    [sGLView addSubview:bannerViewController.view];
	
###unity version 4.x
	// 找到 "CreateViewHierarchy();", 添加
    MyView *myView = [[MyView alloc] init];
    UIViewController *sGLView =  UnityGetGLViewController();
    [sGLView.view addSubview:myView.view];