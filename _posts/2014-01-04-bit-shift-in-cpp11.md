---
layout: post
title: "Bit shift in cpp11"
description: ""
category: 
tags: []
---
{% include JB/setup %}

今天复习一下 c++ 里面的位移操作

	#include <iostream>
	void uc_shift(unsigned char uc)
	{
		uc = uc << 1;
		std::cout<<(unsigned int )uc<<std::endl;
	}
	int main(int argc, char const *argv[])
	{
		unsigned char uc0 {255}; // 11111111
		uc_shift(uc0);			 // 11111110
	 
		uc0 = {252};		 	 // 11111100
		uc_shift(uc0);			 // 11111000

	}