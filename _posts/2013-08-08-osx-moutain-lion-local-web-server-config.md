---
layout: post
title: "OSX Moutain Lion local web server config"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Reference

[discussion apple](https://discussions.apple.com/docs/DOC-3083)

[StackExchage](http://apple.stackexchange.com/questions/23751/how-to-turn-mac-os-x-lion-into-a-web-server)


#Step 1:Hello World
	mkdir ~/Sites
	echo "<html><body><h1>Hello World</h1></body></html>" > ~/Sites/helloword.html

#Step 2:

	sudo vi /etc/apache2/httpd.conf

为下面两行去掉注释

	#LoadModule php5_module libexec/apache2/libphp5.so
	#Include /private/etc/apache2/extra/httpd-vhosts.conf

#Step 3:

	修改:
	/private/etc/apache2/extra/httpd-vhosts.conf

	<VirtualHost *:80>
    	DocumentRoot "/Users/yourname/Sites/"
	</VirtualHost>

#Step 4: config php for mysql

	sudo cp /etc/php.ini.default /etc/php.ini

	修改 /etc/php.ini
	把下面三个参数的目录都设置为 /tmp/mysql.sock
	mysql.default_socket
	mysqli.default_socket
	pdo_mysql.default_socket


#Step 5:
	
	sudo apachectl restart