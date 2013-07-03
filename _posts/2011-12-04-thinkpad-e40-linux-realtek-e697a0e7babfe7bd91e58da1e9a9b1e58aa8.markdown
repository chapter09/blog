---
author: chapter09
comments: true
date: 2011-12-04 16:06:43+00:00
layout: post
slug: thinkpad-e40-linux-realtek-%e6%97%a0%e7%ba%bf%e7%bd%91%e5%8d%a1%e9%a9%b1%e5%8a%a8
title: Thinkpad E40 Linux Realtek 无线网卡驱动
wordpress_id: 240
categories:
- Tech
tags:
- Linux
- Realtet
- Thinkpad
---

同学Thinkpad E40配置到是Realtek 8192ce到无线网卡，自己装好Ubuntu后，无线网卡无法识别。

用ifconfig -a只能得到eth0 和 lo 两个接口，所以判断是驱动没有起来。

<!-- more -->

同时同学自己通过ubuntu本身的Additional Drivers得到到驱动是无法使用到，所以卸掉。

去realtek官网下载源代码自己编译。

**make**

**sudo make install**

然后得到编好到模块rtl8192ce.ko

于是

**modprobe rtl8192ce**

再次ifconfig -a的时候可以看到wlan0了

reboot后其实又得重新modprobe，于是得写配置文件来设置开机加载模块：

在/etc/modules这个文件中加入新到一行:

rtl8192ce即可

这样可以正常使用了。
