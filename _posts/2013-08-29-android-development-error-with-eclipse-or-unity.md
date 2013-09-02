---
layout: post
title: "Android development error with eclipse or unity"
description: ""
category: 
tags: []
---
{% include JB/setup %}

每次用 eclipse 开发安卓都要吐槽, 都和 SDK 有关.

1) eclipse 和 sdk 配合会出现 R.java 无法生成的 bug. 使用最新的 SDK 试试.

2) SDK Manager 在大陆无法正常更新, 必须绕过GFW, 修改 host,添加
	
	// 写这篇日志时有效
	74.125.237.1 dl-ssl.google.com
	
3) 
	
	Failed to compile Java code to DEX:
	 >java -Xmx1024M -Djava.ext.dirs="android-sdk-r22/sdk/platforms/android-18/	lib/" -jar "android-sdk-r22/platforms/android-18/lib/dx.jar" --dex --verbose 	--	output=bin/classes.dex bin/classes.jar plugins
	Unable to access jarfile /Users/tracy/Downloads/adt-bundle-mac-	x86_64-20130729/sdk/platforms/android-18/lib/dx.jar


4) 

	Error building Player: Win32Exception: ApplicationName='/Users/tracy/	Downloads/adt-bundle-mac-x86_64-20130729/sdk/tools/adb', CommandLine='start-	server', CurrentDirectory=''
	
	
5)

    Error building Player: CommandInvokationFailure: Failed to re-package resources. See the Console for details.
    /Users/tracy/Downloads/adt-bundle-mac-x86_64-20130729/sdk/build-tools/android-4.3/aapt package --auto-add-overlay -v -f -m -J gen -M AndroidManifest.xml -S "res" -I "/Users/tracy/Downloads/adt-bundle-mac-x86_64-20130729/sdk/platforms/android-18/android.jar" -F bin/resources.ap_

    stderr[
    AndroidManifest.xml:4: error: Error: No resource found that matches the given name (at 'label' with value '@string/str_app_name').
    AndroidManifest.xml:4: error: Error: No resource found that matches the given name (at 'theme' with value '@style/AppTheme').
    AndroidManifest.xml:5: error: Error: No resource found that matches the given name (at 'label' with value '@string/str_app_name').
    ]
    stdout[
    Configurations:
     (default)
     hdpi
     ldpi
     mdpi
     xhdpi
     xxhdpi

    Files:
      drawable/app_icon.png
        Src: () res/drawable/app_icon.png
        Src: (hdpi) res/drawable-hdpi/app_icon.png
        Src: (ldpi) res/drawable-ldpi/app_icon.png
        Src: (xhdpi) res/drawable-xhdpi/app_icon.png
        Src: (xxhdpi) res/drawable-xxhdpi/app_icon.png
      drawable/ic_launcher.png
        Src: (hdpi) res/drawable-hdpi/ic_launcher.png
        Src: (mdpi) res/drawable-mdpi/ic_launcher.png
        Src: (xhdpi) res/drawable-xhdpi/ic_launcher.png
        Src: (xxhdpi) res/drawable-xxhdpi/ic_launcher.png
      values/strings.xml
        Src: () res/values/strings.xml
      AndroidManifest.xml
        Src: () AndroidManifest.xml

    Resource Dirs:
      Type drawable
        drawable/app_icon.png
          Src: () res/drawable/app_icon.png
          Src: (hdpi) res/drawable-hdpi/app_icon.png
          Src: (ldpi) res/drawable-ldpi/app_icon.png
          Src: (xhdpi) res/drawable-xhdpi/app_icon.png
          Src: (xxhdpi) res/drawable-xxhdpi/app_icon.png
        drawable/ic_launcher.png
          Src: (hdpi) res/drawable-hdpi/ic_launcher.png
          Src: (mdpi) res/drawable-mdpi/ic_launcher.png
          Src: (xhdpi) res/drawable-xhdpi/ic_launcher.png
          Src: (xxhdpi) res/drawable-xxhdpi/ic_launcher.png
      Type values
        values/strings.xml
          Src: () res/values/strings.xml
    Including resources from package: /Users/tracy/Downloads/adt-bundle-mac-x86_64-20130729/sdk/platforms/android-18/android.jar
    applyFileOverlay for drawable
    applyFileOverlay for layout
    applyFileOverlay for anim
    applyFileOverlay for animator
    applyFileOverlay for interpolator
    applyFileOverlay for xml
    applyFileOverlay for raw
    applyFileOverlay for color
    applyFileOverlay for menu
    applyFileOverlay for mipmap
    Processing image: res/drawable/app_icon.png
    Processing image: res/drawable-hdpi/app_icon.png
    Processing image: res/drawable-ldpi/app_icon.png
    Processing image: res/drawable-xhdpi/app_icon.png
        (processed image res/drawable-ldpi/app_icon.png: 98% size of source)
    Processing image: res/drawable-xxhdpi/app_icon.png
        (processed image res/drawable/app_icon.png: 99% size of source)
    Processing image: res/drawable-hdpi/ic_launcher.png
        (processed image res/drawable-hdpi/app_icon.png: 99% size of source)
    Processing image: res/drawable-mdpi/ic_launcher.png
        (processed image res/drawable-mdpi/ic_launcher.png: 100% size of source)
    Processing image: res/drawable-xhdpi/ic_launcher.png
        (processed image res/drawable-hdpi/ic_launcher.png: 100% size of source)
    Processing image: res/drawable-xxhdpi/ic_launcher.png
        (processed image res/drawable-xhdpi/app_icon.png: 99% size of source)
        (processed image res/drawable-xhdpi/ic_launcher.png: 100% size of source)
        (processed image res/drawable-xxhdpi/ic_launcher.png: 100% size of source)
        (processed image res/drawable-xxhdpi/app_icon.png: 98% size of source)
        (new resource id app_icon from drawable/app_icon.png #generated)
        (new resource id app_icon from hdpi/drawable/app_icon.png #generated)
        (new resource id app_icon from ldpi/drawable/app_icon.png #generated)
        (new resource id app_icon from xhdpi/drawable/app_icon.png #generated)
        (new resource id app_icon from xxhdpi/drawable/app_icon.png #generated)
        (new resource id ic_launcher from hdpi/drawable/ic_launcher.png #generated)
        (new resource id ic_launcher from mdpi/drawable/ic_launcher.png #generated)
        (new resource id ic_launcher from xhdpi/drawable/ic_launcher.png #generated)
        (new resource id ic_launcher from xxhdpi/drawable/ic_launcher.png #generated)
    ]