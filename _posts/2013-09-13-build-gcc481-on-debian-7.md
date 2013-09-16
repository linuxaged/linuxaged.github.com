---
layout: post
title: "Build gcc4.8.1 On Debian 7"
description: ""
category: 
tags: []
---
{% include JB/setup %}

[link](http://stackoverflow.com/questions/17409740/python-bigfloat-package-installation-issues)

#安装依赖库

	# Create the desired destination directory for GMP, MPFR, and MPC.
	$ mkdir /home/case/local
	# Download and un-tar the GMP source code. Change to GMP source directory and compile GMP.
	$ cd ~/src/gmp-5.1.0
	$ ./configure --prefix=/home/case/local
	$ make -j 8
	$ make -j 8 check
	$ make install
	# Download and un-tar the MPFR source code. Change to MPFR source directory and compile MPFR.
	$ cd ~/src/mpfr-3.1.1
	$ ./configure --prefix=/home/case/local --with-gmp=/home/case/local
	$ make -j 8
	$ make -j 8 check
	$ make install
	# Download and un-tar the MPC source code. Change to MPC source directory and compile MPC.
	$ cd ~/src/mpc-1.0.1
	$ ./configure --prefix=/home/case/local --with-gmp=/home/case/local --with-mpfr=/home/case/local
	$ make -j 8
	$ make -j 8 check
	$ make install
	
	上面装的3个库，GCC找不到，需要设置变量 LD_LIBRARY_PATH
       ~/.profile文件，在文件的最后加上一行：
       export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib （需先注销，永久有效）
       
#编译 gcc

	$ wget http://www.netgull.com/gcc/releases/gcc-4.8.1/gcc-4.8.1.tar.bz2
	$ tar -xvjf gcc-4.8.1.tar.bz2
	$ cd gcc-4.8.1
	$ ./configure --prefix=/root/gcc481 --with-gmp=/root/gcc481 --with-mpfr=/root/gcc481 --enable-languages=c,c++,go
	$ make