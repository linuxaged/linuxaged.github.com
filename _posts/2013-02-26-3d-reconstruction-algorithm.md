---
layout: post
title: "3d Reconstruction Algorithm"
description: ""
category: 
tags: []
---
{% include JB/setup %}
#Reference
http://docs.opencv.org/modules/calib3d/doc/camera_calibration_and_3d_reconstruction.html

#OpenCV


#Paper
	α=0
	β=90
	get new image from the camera (I1)
	undistort the (I1) image (using intrinsic parameters of camera)
	detect feature points on the image
	store image coordinates of feature points on memory (L)
	for step=1 to 400 ; step=step+1
		get new image from the camera (I2)
		undistort the I2 image
		track feature points on I1 and I2 using optical flow
		store new image coordinates of feature points on memory (R)
		if (step mod 10 = 0)
			3d reconstruction using stored image coordiantes (L and R)
			(See Section 3.2)
			reset feature points (clear all feature points)
			detect feature points on the image
			(new feature points are detected on the same image)
			store feature point image coordinates on memory (L)
		end if
		rotate z-motor one step forward
		α=step*1.8 (orα=α+1.8)
		if (step mod 8 =0 )
			rotate y-motor one step forward
			β = β −1.8
		end if
	end for