---
author: chapter09
comments: true
date: 2012-07-08 12:08:22+00:00
layout: post
slug: tor-brief-info
title: Tor brief info
wordpress_id: 840
categories:
- Tech
tags:
- Tor
---

_from [https://www.torproject.org/about/overview.html](https://www.torproject.org/about/overview.html.en)_

Tor helps to reduce the risks of both simple and sophisticated traffic analysis by distributing your transactions over several places on the Internet, so no single point can link you to your destination.

Instead of taking a direct route from source to destination, data packets on the Tor network take a random pathway through several relays that cover your tracks so no observer at any single point can tell where the data came from or where it's going.

实际上Tor是一个混淆层，正如其文档里说的，Tor所坐的就是通过七拐八拐帮你把跟踪你的人甩掉，所谓匿名网络就是隐匿了packet的来源，最终packet仍然是需要从Tor虚拟电路(Virtual Circuit)出来，然后通过非安全的网路达到目的机器。

Tor能够在一定时间以后就更新Tor虚拟电路中的路由路径，同时Tor网络中的节点只知道上一个节点和下一个节点. 而一个节点是从上一个节点发来的信息中获得下一个节点的信息。

encrypted onion:

[![](http://haow.ca/wp-content/uploads/2012/07/Onion_diagram.png)](http://haow.ca/wp-content/uploads/2012/07/Onion_diagram.png)

洋葱是这样来的：

发送者从Tor电路中获得一定量节点后，组成一个path。发送者通过这些节点的公钥，反向由path的顺序对信息message进行一层层加密。

然后加密洋葱发出去以后，过一个节点就将被节点上的私钥解密，获得下一个发往节点的信息。最后在发往目的节点的前一个节点，洋葱皮将被剥完。

但是If there is end-to-end encryption between the sender and the recipient, then not even the last intermediary can view the original message; this is similar to a game of '[pass the parcel](http://en.wikipedia.org/wiki/Pass_the_parcel)'.

[以上信息部分来自Wiki：[http://en.wikipedia.org/wiki/Onion_routing](http://en.wikipedia.org/wiki/Onion_routing)] 
