---
author: chapter09
comments: true
date: 2012-05-04 11:31:42+00:00
layout: post
slug: understand-lvm
title: Understand LVM
wordpress_id: 673
categories:
- Tech
tags:
- Linux
- LVM
---

## **[Terminology]**


Physical Volume:

the partition of physical disk, like /dev/sda1, /dev/hd2. But PV contains the information of LVM, such asVolume Group the PV belongs to.

Volume Group:

We get PVs from physical hard disk, and there PVs can be formed into a VG, this VG can be treated as an whole disk.

Logical Volume:

On the VG we create, we could create Logical Volumes, which are just like the physical partition on hard disk.

![](http://a1.att.hudong.com/45/30/01300000089793120686306318932.jpg)


## **[example]**


1. create a VG


> vgcreate fileserver /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1


2. create LV


> lvcreate --name share --size 40G fileserver






for more detailed tutorial on LVM, please click [http://www.howtoforge.com/linux_lvm](http://www.howtoforge.com/linux_lvm)

Examples of this post also comes from the tutorial above.
