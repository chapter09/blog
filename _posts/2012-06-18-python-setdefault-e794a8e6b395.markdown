---
author: chapter09
comments: true
date: 2012-06-18 06:56:48+00:00
layout: post
slug: python-setdefault-%e7%94%a8%e6%b3%95
title: python setdefault 用法
wordpress_id: 780
categories:
- Tech
tags:
- Python
---

[code lang="python"]
def args(*args, **kwargs):
    def _decorator(func):
        func.__dict__.setdefault('options', []).insert(0, (args, kwargs))
        return func
    return _decorator
[/code]

setdefault(key[, default])
If key is in the dictionary, return its value. If not, insert key with a value of default and **return default**. default defaults to None.

之前在想为什么在decorator中常有函数wrap的用法？这是为什么？直接返回func不好么？
的确有所不好：
对于decorator可以理解成：
func = deco(deco_args)func(args)
实际上希望返回给func的仍然是一个callable的函数，在一下代码中：
[code lang="python"]
def makebold(fn):
    def wrapped():
        return "<b>" + fn() + "</b>"
    return wrapped

def makeitalic(fn):
    def wrapped():
        return "<i>" + fn() + "</i>"
    return wrapped

@makebold
@makeitalic
def hello():
    return "hello world"

print hello() ## returns <b><i>hello world</i></b>
[/code]
如果直接返回return "<b>" + fn() + "</b>"将得到TypeError: 'str' object is not callable的错误
因此一般在deco中还是用wrapper的形式
在闭包的用法中，应该也是如此
