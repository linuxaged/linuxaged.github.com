---
layout: post
title: "OpenCV xcode build"
description: ""
category: 
tags: []
---
{% include JB/setup %}
在 osx 下构建 OpenCV 开发环境
	OSX 10.8.2
	XCode 4.6

1.获取源码

	git clone https://github.com/Itseez/opencv.git


2.构建
	mkdir build
	cd build
	cmake -G "Unix Makefiles" ..
	make -j8
	sudo make install
  如果构建成功,库和头文件会被放置在 /usr/local/ 目录下

3.新建工程

	-新建 command line 工程
	-在 Targets -> Build Phases 下添加库文件 libopencv_core_2.4.9.dylib libopencv_highgui_2.4.9.dylib libopencv_imgproc_2.4.9.dylib
	-在 Build Settings下设置 Header Search Paths 并且设为 recusive
	-用下面这段代码替换 main.cpp
			// Example showing how to read and write images
			#include <opencv2/opencv.hpp>
			#include <opencv2/highgui/highgui.hpp>
			#include <opencv/cvaux.hpp>
			int main(int argc, char** argv)
			{
			    IplImage * pInpImg = 0;
    
			    // Load an image from file - change this based on your image name
			    pInpImg = cvLoadImage("logo.jpg", CV_LOAD_IMAGE_UNCHANGED);
			    if(!pInpImg)
			    {
			        fprintf(stderr, "failed to load input image\n");
			        return -1;
			    }
    
			    // Write the image to a file with a different name,
			    // using a different image format -- .png instead of .jpg
			    if( !cvSaveImage("logo_copy.png", pInpImg) )
			    {
			        fprintf(stderr, "failed to write image file\n");
			    }
    
			    // Remember to free image memory after using it!
			    cvReleaseImage(&pInpImg);
    
			    return 0;
			}


4.编译错误

	‘cvaux.h’ file not found with <angled> include; use "quotes" instead.
  解决:
  
	#include <cvaux.h>
  替换成
	#include "cvaux.h"