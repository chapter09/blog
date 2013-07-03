---
author: chapter09
comments: true
date: 2012-05-13 18:04:56+00:00
layout: post
slug: how-to-import-libs-in-two-versions-under-python
title: How to import libs in two versions under Python
wordpress_id: 685
categories:
- Tech
tags:
- Python
---

的确这是一个奇怪的需求

但是假设在一个project里需要调用的函数正好在一个库的两个不同版本里，因为版本change的关系，有些函数不再支持。

这样看来这种需求就是合理的。更重要的是，希望弄清楚Python的命名空间以及import寻址问题。

目前看到的一个方法是：

mkdir lib_v1

mkdir lib_v2

然后将一个module的不同版本各自扔进去，在添加path

而后使用的时候这样：

import lib_v1.module

import lib_v2.module
