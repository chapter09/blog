---
author: wanghao
comments: true
date: 2013-03-29 09:13:13+00:00
layout: post
slug: run-android-adb-on-64bit-linux
title: run Android ADB on 64bit Linux
wordpress_id: 988
categories:
- Tech
tags:
- Android
---

After I update all the necessary data of Android SDK, the AVD manager still could not start the Android VM, after search the Internet, it is said that I need to install

[code]
sudo apt-get install ia32-libs
[/code]


But, actually this package could not be installed via apt-get directly.

After install packages as follow, the Android VM and ADB works normally:

[code]
sudo apt-get install libc6:i386 libgcc1:i386 gcc-4.6-base:i386 libstdc++5:i386 libstdc++6:i386

sudo apt-get install libstdc++6:i386 libgcc1:i386 zlib1g:i386 libncurses5:i386

sudo apt-get install libsdl1.2debian:i386
[/code]
