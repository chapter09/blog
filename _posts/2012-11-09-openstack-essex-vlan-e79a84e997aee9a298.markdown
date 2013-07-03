---
author: chapter09
comments: true
date: 2012-11-09 15:24:04+00:00
layout: post
slug: openstack-essex-vlan-%e7%9a%84%e9%97%ae%e9%a2%98
title: OpenStack Essex Vlan 的问题
wordpress_id: 915
categories:
- Tech
tags:
- openstack
---

环境：


Controller: manage_ip 192.168.3.103
Compute: 192.168.3.101
vlan-id: 100
dell-swtich: vlan port mode: Access


问题：


在nova-compute节点上VM能够正常启动，在Console.log下可以看到：
> udhcpc (v1.17.2) started
> Sending discover...
> Sending discover...
> Sending discover...
>


解决：

把swtich vlan port 模式改为Trunk即可

要补补网络知识了。。。

