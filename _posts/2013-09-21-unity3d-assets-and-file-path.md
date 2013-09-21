---
layout: post
title: "Unity3D Assets and File Path"
description: ""
category: 
tags: []
---
{% include JB/setup %}

读写资源是游戏开发者的基本功.我表示还没掌握好, 面壁思过…


#Application.dataPath
顾名思义, 就是最后程序包中的资源路径, 具体来说,

	Mac 	-> /Assets
	iOS 	-> /Contents
	Windows -> /Data
	Android -> ?
#Application.persistentDataPath
这个目录是和 bundle, GUID 相关的, 比如 com.company0.app0.xkl4l1af2h1auhdfjhl
#BinaryReader
以字节形式读取文件

	// 文件名叫做 binFile.txt
	TextAsset asset = Resources.Load("binFile") as TextAsset;	Stream s = new MemoryStream(asset.bytes);
	BinaryReader br = new BinaryReader(s);
	
#AssetDatabase
这个是 Editor 专用
#[FileStream](http://msdn.microsoft.com/zh-tw/library/system.io.filestream.aspx)