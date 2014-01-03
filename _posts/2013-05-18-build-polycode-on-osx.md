---
layout: post
title: "Build Polycode on OSX"
description: ""
category: 
tags: []
---
{% include JB/setup %}

update 2013/11/23

#Reference
[Tutorial building Ploycode On Mac](http://polycode.org/wiki/index.php?n=Main.TutorialBuildingPolycodeOnMac)

#Errors

1. can not find header or static lib : both PolycodeDependencies and Polycode Xcode projects' **install** scheme need to be built
2. link error : set the c++ standard lib to libstdc++
3.  libpng error(runtime) : delete the Mono.framework, then rebuild the Polycode xcode project
