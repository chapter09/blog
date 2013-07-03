---
author: chapter09
comments: true
date: 2012-03-05 03:06:54+00:00
layout: post
slug: u%e7%9b%98%e5%bc%95%e5%af%bc%e7%9a%84%e4%b8%80%e4%b8%aa%e5%bf%85%e8%a6%81%e6%ad%a5%e9%aa%a4under-win
title: U盘引导的一个必要步骤(under Win)
wordpress_id: 614
categories:
- Tech
tags:
- U 盘引导
---

在光驱即将消失的今天，使用U盘安装各种系统是必然步骤。

水果家的上一个版本系统就是推出一款Apple U盘来进行系统安装的。

但是常常会发生U盘安装系统时，引导错误的情况，为什么？

因为还需要以下步骤：


1. 首先我们需要找到一个4GB大小的U盘然后将它插在计算机上。
2. 打开屏幕左下角的“开始菜单”，然后运行“cmd”命令，紧接着你会看见一个控制窗口弹出。
3. 然后在窗口中输入以下内容：




* 首先键入‘diskpart’命令；
* ‘list disk’：敲入该命令系统将自动列出所有电脑上发现的磁盘，包括我们刚刚插上去的U盘。
* 此时可以看到U盘所对应的编号（如#），紧接着键入：‘select disk #’；
* 然后键入‘clean’；
* 键入‘create partition primary’；
* 然后输入‘select partition 1’；
* 输入‘active’；
* 再输入‘format fs=fat32 quick’。
