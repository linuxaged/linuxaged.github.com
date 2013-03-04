---
layout: post
title: "OpenCV Automatic Number Plate Recognition using SVM and Neural Networks"
description: ""
category: 
tags: []
---
{% include JB/setup %}
#ANPR algorithm
1.plate detection
2.plate recognition

#Plate detection
• Sobel filter
• Threshold operation
• Close morphologic operation
• Mask of one filled area
• Possible detected plates marked in red (features images)
• Detected plates after the SVM classifier

#Segmentation
	   //convert image to gray
	   Mat img_gray;
	   cvtColor(input, img_gray, CV_BGR2GRAY);
	   blur(img_gray, img_gray, Size(5,5));

	   用 Sobel 滤波器查找垂直边线
	   //Find vertical lines. Car plates have high density of vertical lines
	   Mat img_sobel;
	   Sobel(img_gray, img_sobel, CV_8U, 1, 0, 3, 1, 0);
	   
	   获取黑白图
	   //threshold image
	   Mat img_threshold;
	   threshold(img_sobel, img_threshold, 0, 255, CV_THRESH_OTSU+CV_THRESH_
	   BINARY);

#编译chapter5
在之前 2013-02-28-opencv-xcode-build 工程的基础上构建 ANPR 项目

1.导入项目文件
2.设置 c++ Standard Library 为 libstdc++(GNU c++ standard library)
3.lsh_table.h 文件中 
	#if USE_UNORDERED_MAP
	#include <unordered_map>
	#else

  改为
	#if USE_UNORDERED_MAP
	#include <tr1/unordered_map>
	#else

http://stackoverflow.com/questions/7359284/unordered-map-error-in-gcc
http://stackoverflow.com/questions/8454329/why-cant-clang-with-libc-in-c0x-mode-link-this-boostprogram-options-examp