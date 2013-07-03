---
author: chapter09
comments: true
date: 2012-05-09 08:49:26+00:00
layout: post
slug: usb2-0-nic-on-linux-2-6-22-above
title: USB2.0 NIC on linux 2.6.22 above
wordpress_id: 682
categories:
- Tech
tags:
- Linux
- NIC
---

To install ASIX AX88xxx Based USB 2.0 Ethernet Adapters on linux 2.6.39 RHEL6.1, first I tried to build an older kernel 2.6.26 as the ReadMe file in the driver package, but it seems the current OS is on EXT4 disk format, so 2.6.26 supports EXT4 badly, which caused the installation pause. So the NIC driver could not be maked.

Actually the linux kernel above 2.6.22 has already been added into the kernel, so just insmod the asix mode, the NIC will be available.

Detailed information comes from [http://cateee.net/lkddb/web-lkddb/USB_NET_AX8817X.html](http://cateee.net/lkddb/web-lkddb/USB_NET_AX8817X.html)


