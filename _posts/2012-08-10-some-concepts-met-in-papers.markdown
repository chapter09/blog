---
author: chapter09
comments: true
date: 2012-08-10 15:14:05+00:00
layout: post
slug: some-concepts-met-in-papers
title: Some concepts met in papers
wordpress_id: 863
categories:
- Tech
---

1. Bloom Filter

A Bloom filter is a data structure designed to tell you, rapidly and memory-efficiently, whether an element is present in a set.

Bloom Filter是一种空间效率很高的随机数据结构，它利用位数组很简洁地表示一个集合，并能判断一个元素是否属于这个集合。Bloom Filter的这种高效是有一定代价的：在判断一个元素是否属于某个集合时，有可能会把不属于这个集合的元素误认为属于这个集合（false positive）。因此，Bloom Filter不适合那些“零错误”的应用场合。而在能容忍低错误率的应用场合下，Bloom Filter通过极少的错误换取了存储空间的极大节省。

[http://llimllib.github.com/bloomfilter-tutorial/](http://llimllib.github.com/bloomfilter-tutorial/)

[http://blog.csdn.net/zzcase/article/details/6623424](http://blog.csdn.net/zzcase/article/details/6623424)

2.
