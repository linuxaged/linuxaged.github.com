---
layout: post
title: "Compile PlaneShift on OSX"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Prepare tools and source code
##Tools
make sure you have `jam`, `curl`, `svm` installed.
##Planeshift
`svn co https://planeshift.svn.sourceforge.net/svnroot/planeshift/trunk planeshift`
##Cal3D
`svn co -r 507 http://svn.gna.org/svn/cal3d/trunk/cal3d cal3d`


#编译
##Server 需要使用 gcc
[link](http://www.crystalspace3d.org/docs/online/manual-2.0/MacOS_002fX-Requirements.html)
因为 XCode 4.2+ 不支持 virtual variadic functions.
###安装 gcc 4.8
http://apple.stackexchange.com/questions/38222/how-do-i-install-gcc-via-homebrew

            ls /usr/local/Cellar/gcc48/4.8.1/bin 
            c++-4.8
            cpp-4.8
            g++-4.8
            gcc-4.8
            gcc-ar-4.8
            gcc-nm-4.8
            gcc-ranlib-4.8
            gcov-4.8
            x86_64-apple-darwin12.3.0-c++-4.8
            x86_64-apple-darwin12.3.0-g++-4.8
            x86_64-apple-darwin12.3.0-gcc-4.8
            x86_64-apple-darwin12.3.0-gcc-4.8.1
            x86_64-apple-darwin12.3.0-gcc-ar-4.8
            x86_64-apple-darwin12.3.0-gcc-nm-4.8
            x86_64-apple-darwin12.3.0-gcc-ranlib-4.8
            
###编译
`./configure --prefix=$HOME/workspace/cal3d CC=/usr/local/bin/gcc-4.8 CXX=/usr/local/bin/g++-4.8`

`./configure --enable-debug --without-java --without-perl --without-python --without-3ds --with-cal3d=$HOME/workspace/cal3d CC=/usr/local/bin/gcc-4.8 CXX=/usr/local/bin/g++-4.8 OBJCXX=/usr/bin/clang++ OBJC=/usr/bin/clang`

`./configure --enable-debug --with-cal3d=$HOME/workspace/cal3d CC=/usr/local/bin/gcc-4.8 CXX=/usr/local/bin/g++-4.8`
##cal3d
报错：

`configure.in:16: error: 'AM_CONFIG_HEADER': this macro is obsolete.
    You should use the 'AC_CONFIG_HEADERS' macro instead.`

解决：
按错误提示替换成 “`AC_CONFIG_HEADERS`” 即可

又见错误：

`configure.in:16: error: possibly undefined macro: AM_CONFIG_HEADERS`

解决：

确保安装了 libtool

`brew install libtool`

`brew link libtool`

##Crytal Space
http://www.crystalspace3d.org/docs/online/manual-2.0/MacOS_002fX-Requirements.html