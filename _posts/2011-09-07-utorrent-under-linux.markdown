---
author: chapter09
comments: true
date: 2011-09-07 16:15:53+00:00
layout: post
title: uTorrent under Linux
categories:
- Tech
tags:
- Linux
- PT
- torrent
---

Win下的uTorrent的确好用，轻量高效。

之前一直在Linux下使用Vuze(毒蛙)，Vuze并不太稳定，总之我Ubuntu 11.04下偶尔就直接消失在前台，然后后台占着资源和带宽无法控制，只好直接kill。

<!-- more -->

uTorrent这边今天发现可以在Linux下使用了，不太麻烦也不太方便。采用是在Linux下跑一个后台服务，然后浏览器访问得到界面：



	
  1. 下载uTorrent包，[http://www.utorrent.com/downloads/complete?os=linux](http://www.utorrent.com/downloads/complete?os=linux)

	
  2. 解压，运行utserver

	
  3. 浏览器访问[http://localhost:8080/gui/](http://localhost:8080/gui/)，用户名admin，密码空

	
  4. 然后就可以像在Win下一样了


[![](http://haow.ca/wp-content/uploads/2011/09/Screenshot.png)](http://haow.ca/wp-content/uploads/2011/09/Screenshot.png)
