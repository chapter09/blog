---
author: chapter09
comments: true
date: 2012-11-29 10:37:03+00:00
layout: post
slug: c-struct-defined-in-main
title: '[C++] struct defined in main()'
wordpress_id: 927
categories:
- Tech
tags:
- C++
---

[code lang="c"]
#include
#include
#include
#include <map>
using namespace std;

int main ()
{
struct lstr {
int a;
int b;
};
lstr ll = {1, 2};
map tmap;
tmap[1] = ll;
printf("1 means %d %d n", tmap[1].a, tmap[1].b);
tmap[1].a = 5;
tmap[1].b = 7;
printf("1 means %d %d n", tmap[1].a, tmap[1].b);
return 0;
}
[/code]




上面代码运行的结果是
struct.cc: In function ‘int main()’:
struct.cc:19: error: template argument for ‘template struct std::pair’ uses local type ‘main()::lstr’

为什么在struct必须放到main函数以外才是正确呢？
但是在类定义的时候，struct 的定义和map的定义可以放在同一个作用域内。


> 3feng告诉我说：struct相当于类，而类不能定义在函数里面
