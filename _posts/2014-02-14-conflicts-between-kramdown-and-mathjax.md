---
layout: post
title: "Conflicts between Kramdown and MathJax"
description: ""
categories: 
- Tech
tags: []
---
{% include JB/setup %}

Kramdown里自己定义的Math Block是由 `$$` 来作为Mark来render的，而对于MathJax，`$$`是$\LaTeX$ 块，而`$$$`是inline的，因此只能通过改MathJaX的config了。

具体参考以下来源

[link](http://www.lucypark.kr/blog/2013/02/25/mathjax-kramdown-and-octopress/)