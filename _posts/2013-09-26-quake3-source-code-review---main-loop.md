---
layout: post
title: "Quake3 source code review   main loop"
description: ""
category: 
tags: []
---
{% include JB/setup %}

	main(){
	…
	while( 1 )
	{
		IN_Frame( );
		Com_Frame( );
	}
	…
	