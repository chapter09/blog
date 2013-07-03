---
author: chapter09
comments: true
date: 2012-02-08 07:53:02+00:00
layout: post
slug: android-%e4%b8%8d%e7%bf%bb%e5%a2%99%e4%b8%8atwitter
title: 'Android 不翻墙上Twitter  '
wordpress_id: 439
categories:
- Tech
tags:
- GFW
- KilltheWall
- Proxy API
- Twitter
- 翻墙
---

不论是走VPN/SSH Tunnel，或是走××功的××门，还是改hosts，还是用Proxy API，大家都在摸着滚着上Twitter。但是对于移动设备来说使用Proxy API是最好的方式（尽管会需要时不时地改API），比较你手机不可能一直开着VPN，或者连着SSH。<!-- more -->

下载软件Seesmic, Add an account -> Twitter Proxy API:

![](http://haow.ca/wp-content/uploads/2012/02/20120208144005-168x300.png)![](http://haow.ca/wp-content/uploads/2012/02/20120208144025-168x300.png)

用户名密码照常

然后API Server和Search API Server一般就是一个。另外可能需要取消掉Use XAuth那个选项。

Sign in即可。当然你的Seesmic可能需要你先手机连上VPN通过Twitter的软件认证。

[![](http://haow.ca/wp-content/uploads/2012/02/20120208023852-168x300.png)](http://haow.ca/wp-content/uploads/2012/02/20120208023852.png)

**

#### [API ****怎么来？]

**

**你可以google，有人在GAE上架设后开放出来。你也可以用twip自己架设。**

**[P.S.]**

另外国产软件YiBo也支持API上Twitter
