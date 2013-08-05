---
layout: post
title: "c funtion paramater"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#c 函数参数传递机制

考虑下面的代码

		void func1(int *poiIn)
		{
    		poiIn = malloc(512);
		}

		int main(int argc, const char * argv[])
		{
    		int *poi1 = NULL;
    		func1(poi1); // 因为是拷贝传递，执行完，poi1 还是为 NULL
    		return 0;
		}



		void func1(int **poiIn)
		{
    		*poiIn = malloc(512);
		}

		int main(int argc, const char * argv[])
		{
    		int *poi1 = NULL;
    		func1(&poi1); // 因为是指针传递，执行完，poi1指向堆上内存地址
    		return 0;
		}
