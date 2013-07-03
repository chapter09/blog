---
author: chapter09
comments: true
date: 2012-09-29 06:27:20+00:00
layout: post
slug: look-into-the-details
title: Look into the details
wordpress_id: 895
categories:
- Tech
tags:
- Paper
---

[GFS]


> If a write by the application is large or straddles a chunkboundary, GFS client code breaks it down into multiplewrite operations. They all follow the control flow describedabove but may be interleaved with and overwritten by con-current operations from other clients. Therefore, the sharedfile region may end up containing fragments from differentclients, although the replicas will be identical because the in-dividual operations are completed successfully in the sameorder on all replicas.


当写入的文件超过chunk边际时，大文件的写入将会被切割成多写入操作执行。这些写入操作会依次在控制流中执行，但是与此同时来自其他的client端提出的写操作，会夹杂在这些写操作中，以至于共享的文件区域会包含来自其他不同客户端的碎片
