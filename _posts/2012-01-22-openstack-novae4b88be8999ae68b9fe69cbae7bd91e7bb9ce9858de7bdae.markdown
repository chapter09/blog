---
author: chapter09
comments: true
date: 2012-01-22 10:45:43+00:00
layout: post
slug: openstack-nova%e4%b8%8b%e8%99%9a%e6%8b%9f%e6%9c%ba%e7%bd%91%e7%bb%9c%e9%85%8d%e7%bd%ae
title: 'OpenStack Nova下虚拟机网络配置  '
wordpress_id: 375
categories:
- Tech
tags:
- KVM
- nova
- openstack
---

** Background:**

[OpenStack](http://openstack.org/)提供开放源码软件，建立公共和私有云。 OpenStack是一个社区和一个项目，以及开放源码软件，以帮助企业运行的虚拟计算或者存储云。

OpenStackd开源项目由社区维护，包括OpenStack计算（代号为Nova），OpenStack对象存储（代号为SWIFT），并OpenStack镜像服务（代号Glance）的集合。 OpenStack提供了一个操作平台，或工具包，用于编排云。

<!-- more -->

笔者目前所用的开发平台是OpenSuse提供的一个解决方案openSuse for OpenStack。本文这里涉及的主要还是对nova下KVM虚拟机网络配置以及网桥的理解。对云技术的使用暂且不谈。


| 遇到的问题：




| 通过virsh create yourConfig.xml的方式手动创建的虚拟机连接虚拟机子网失败。




| 虚拟机镜像：RedHat 6.1




| 方式：网桥


**网桥**

** **一开始不太理解网桥，学过后没记牢。

实际上网桥是交换机的前身，交换机是多端口的网桥。两者都工作在数据链路层，因此比工作在网络层的路由器更底层。

网桥和交换机一样根据MAC地址表与相应端口映射来工作，可以使用转发等功能。

从外部上看来，网桥可以从数据链路层这一层面上将不同的网给桥接起来。

在Linux下有命令管理网桥: brctl

有[文章](http://blog.chinaunix.net/space.php?uid=24148050&do=blog&id=193797)介绍如何使用建立配置网桥。

**解决**

由于物理服务器N20里的网桥和DHCP服务器是Nova自动建立的，所以DHCP对手动启动的虚拟机分配IP的请求不会理会，因此这就是为什么改好ifcfg-eth0中BOOTPROTO=dhcp后，restart 网络没有成功

所以对于这种情况干脆手动配置静态网络，这样可以成功连接到虚拟机子网。

**虚拟机子网**

可以通过网桥将虚拟机IP配置到物理服务器的同一网段，也可以内建子网网段

在物理机上，当新建虚拟机时一般会配置相应的虚拟网卡，这个虚拟网卡设备就成了vnet#，这对应着虚拟机中eth#。

使用brctl：

# brctl addif br0 eth0 (让eth0成为br0的一个端口)

这里有一篇博文可以推荐阅读

[http://8366.iteye.com/blog/1000575](http://8366.iteye.com/blog/1000575)

**Redhat6.1系统网络配置**

**在/etc/sysconfig/network-scripts/下修改配置文件即可，配置文件名对应相应的设备。比如：**

对于eth0，则vim ifcfg-eth0文件，写入：

DEVICE=”eth0”

ONBOOT=”yes”

IPADDR=192.168.4.202

NETMASK=255.255.255.0

BOOTPTOTO=”static”   //可以是静态，也可以填dhcp的动态IP

然后#service network restart
