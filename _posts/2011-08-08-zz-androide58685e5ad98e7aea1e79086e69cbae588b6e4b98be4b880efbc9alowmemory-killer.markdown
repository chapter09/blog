---
author: chapter09
comments: true
date: 2011-08-08 01:16:47+00:00
layout: post
title: '[zz] Android内存管理机制之一：lowmemory killer'
categories:
- Tech
tags:
- Android
- Memory
---

from [http://blog.csdn.net/rio2012/article/details/6253165](http://blog.csdn.net/rio2012/article/details/6253165)

（1）Android是一个多任务系统，也就是说可以同时运行多个程序，这个大家应该很熟悉。一般来说，启动运行一个程序是有一定的时间开销的，因 此为了加快运行速度，当你退出一个程序时，Android并不会立即杀掉它，这样下次再运行该程序时，可以很快的启动。随着系统中保留的程序越来越多，内 存肯定会出现不足，这个时候Android系统开始挥舞屠刀杀程序。这里就有一个很明显的问题，杀谁？

（2）Android系统中杀程序 的这个刽子手被称作"LowMemory Killer"，它是在Linux内核中实现的。这里它实现了一个机制，由程序的重要性来决定杀谁。通俗来说，谁不干活，先杀谁。Android将程序的 重要性分成以下几类，按照重要性依次降低的顺序：

名称


oom_adj


解释






FOREGROUD_APP


0


前台程序，可以理解为你正在使用的程序






VISIBLE_APP


1


用户可见的程序






SECONDARY_SERVER


2


后台服务，比如说QQ会在后台运行服务






HOME_APP


4


HOME，就是主界面






HIDDEN_APP


7


被隐藏的程序






CONTENT_PROVIDER


14


内容提供者，






EMPTY_APP


15


空程序，既不提供服务，也不提供内容




其中每个程序都会有一个oom_adj值，这个值越小，程序越重要，被杀的可能性越低。

（3）除了上述程序重要性分类之外，Android系统还维护着另外一张表，这张表是一个对应关系，以N1为例：








oom_adj


内存警戒值( 以4K为单位）






0


1536






1


2048






2


4096






7


5120






14


5632






15


6144




这个表是定义了一个对应关系，每一个警戒值对应了一个重要性值，当系统的可用内存低于某个警戒值时，就杀掉所有大于该警戒值对应的重要性值的程序。 比如说，当可用内存小于6144 * 4K = 24MB时，开始杀所有的EMPTY_APP，当可用内存小于5632 * 4K = 22MB时，开始杀所有
的CONTENT_PROVIDER和EMPTY_APP。

(4) alter minfree改的是什么呢，上面这张对应表是由两个文件组成的：
/sys/module/lowmemorykiller/parameters/adj和/sys/module/lowmemorykiller/parameters/minfree。

alter minfreee就是修改/sys/module/lowmemorykiller/parameters/minfree这个文件的，举例来说，如果把最后一项改为32 * 1024，那么当可用内存小于128MB是，就开始杀所有的EMPTY_APP。


