---
author: chapter09
comments: true
date: 2011-12-01 15:24:59+00:00
layout: post
slug: archlinuxati-radeon-%e4%b8%8d%e8%af%86%e5%88%ab%e6%8a%95%e5%bd%b1%e6%9c%ba%e7%9a%84%e9%97%ae%e9%a2%98
title: Archlinux(ATI Radeon) 不识别投影机的问题
wordpress_id: 236
categories:
- Tech
tags:
- ArchLinux
- 投影机
---

_开组会投影机不识别。。。防止忘记，故记之_

显卡：

01:00.0 VGA compatible controller: ATI Technologies Inc Mobility Radeon HD 3400 Series

<!-- more --><ATI Mobility Radeon HD系显卡应该都差不多>

OS:

Linux  brian 3.1.2-1-ARCH #1 SMP PREEMPT Tue Nov 22 09:17:56 CET 2011 x86_64  Intel(R) Core(TM)2 Duo CPU P8400 @ 2.26GHz GenuineIntel GNU/Linux

问题：

跟投影机八字不合，实际上一方面是没有设置VGA接口信号输出，一方面没有输出投影机能接受的分辨率，这个一般是1024x768或者800x600

解决：

[http://www.thinkwiki.org/wiki/Xorg_RandR_1.2](http://rrurl.cn/258AnT)

通过外接屏幕测试成功，输出分辨率修改很方便


### Using xrandr to do useful things


In  general the commands will specify the output name and either --off or  --auto. In the examples here the external screen is named _VGA_, as used by the Intel driver, with an ATI card the name will probably be _VGA-0_. In general use `$ xrandr -q` to discover the appropriate output names for your configuration. The  --auto option will select the preferred resolution for each output, this  is starred(*) in the `$ xrandr -q` listing and is normally the best resolution available. It is also possible to set a particular mode eg --mode 1024x768.

First clone the two screens, (the smaller screen will display the top left portion of the virtual screen)

`$ xrandr --output LVDS --auto --output VGA --auto --same-as LVDS`

To turn off the VGA monitor.

`$ xrandr --output VGA --off `

_Note: This apparently reboots X !!! - cf. [[1]](http://rrurl.cn/oMZVkN)_

To turn the VGA monitor back on, with its viewport to the right of the laptop monitor:

`$ xrandr --output VGA --auto --right-of LVDS`

This will probably give an error message similar to:

xrandr: screen cannot be larger than 1600x1600 (desired size 2624x1200)

This can be fixed by editing xorg.conf and changing the _virtual_ line (see example above) to something like:

Virtual 2624 1200

Note  that the maximum supported size of the virtual desktop for the Intel  945GM series of chipset with 3D acceleration enabled, is 2048x2048. The  virtual screen can be larger but DRI will be disabled. This may matter  if you like games and compiz desktop effects, or if you want Google  Earth to display in better than geological time. Obviously the larger  the virtual desktop, the more graphics memory is used. So for good  performance with a shared graphics system such as Intel the Virtual  should be no larger than necessary.

It is possible to set screen locations as _--left-of_, _--right-of_, _--above_ and _--below_. Assuming displays sizes of 1024x768 and 1200x1600:

`$ xrandr --output LVDS --auto --output VGA --auto --right-of LVDS`and`$ xrandr --output LVDS --mode 1024x768 --pos 0x0 --output VGA --mode 1600x1200 --pos 1024x0`

are equivalent. Both will place the external monitor to the right of the laptop display within the virtual screen.

If  the Virtual size is only 2048 wide the above command will fail as the  combined width of the two displays exceeds the maximum virtual size.  However it is possible to have overlap the display viewports. So to fit  within the 2048 limit:

`$ xrandr --output VGA --mode 1024x768 --pos 0x0 --output VGA --mode 1600x1200 --pos 448x0`
