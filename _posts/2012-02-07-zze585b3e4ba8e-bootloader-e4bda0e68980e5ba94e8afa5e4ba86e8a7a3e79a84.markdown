---
author: chapter09
comments: true
date: 2012-02-07 10:29:41+00:00
layout: post
slug: zz%e5%85%b3%e4%ba%8e-bootloader-%e4%bd%a0%e6%89%80%e5%ba%94%e8%af%a5%e4%ba%86%e8%a7%a3%e7%9a%84
title: '[zz]关于 Bootloader 你所应该了解的  '
wordpress_id: 423
categories:
- Tech
tags:
- BootLoader
- Milestone
- ROM
---

[文章转载自：**谷安——谷奥Android专题站** [[http://android.guao.hk](http://android.guao.hk/)]]

[caption id="attachment_415" align="aligncenter" width="299" caption="HTC Nexus 解锁BootLoader "] 


![HTC Nexus 解锁BootLoader](http://android.guao.hk/wp-content/uploads/2011/06/5819427279_aeba1ba989.jpg)




[/caption]



我们对于能够及时阻止HTC对其Android设备的Bootloader加锁一事很happy，<!-- more -->也对制造商能够聆听少数用户的声音，并意识到解锁 Bootloader可以提升其产品价值的做法感到非常高兴。我们将持续追踪这件事的进展和效果。同时，我们也收到很多关于这Bootloader为啥引 起这么大聒噪之类的疑问，于是就有了这篇试图解答这个问题的文章，此文试图用不Geek的方式来解答，来壶茶，且品且读吧～ 



* * *





### 谁会关心Bootloader啊、hboots啊刷机啊神马的？


很少的一些人，不过这得看你从神马角度来看这个问题了。现在有40万部/天的Android设备被激活，这些人中的大部分都对（或者压根不关 心）Bootloader是神马毫无概念。这些人可能是个妹纸正在做头发的时候给人发短信；或者是个爷们儿正拿着购物单买螺栓；或者是个得瑟的哥们正在星 巴巴拿着EVO 4G装13。Android现在是主流手机，而你，正在慢慢阅读这篇文章，就说明你比其他一些Android用户更高级更牛13。
正因为关心Bootloader的人非常的小众，所以诸如HTC啊、Moto啊神马的厂商 才敢于动锁Bootloader的心思。但是也有公司敢于跳出这个锁Bootloader的俱乐部，比如Sony Ericsson和HTC实际上改变了他们原来的策略，即使对于少数要求解锁Bootloader的呼声(Moto你听见了么)，他们也正视了这个问题， 正准备提供一个开放的Bootloader，来迎合所有的消费者。

对于那些非常在意Bootloader这桩子事情的爷们和妹纸，他们只是想对手机有完全的掌控权，他们可能是程序猿、主题制作者、开发人员或者甚至 是手机黑客————这些牛13的人总想着能够一点一点的榨干手机的性能，或者让手机变的更好。这些人在网上很活跃，以至于我们这群用户总认为，黑客 Geek才是Android用户群的主体，而事实上，不是这样的。（译者注：此话不假⋯⋯UK街头N多奇形怪状的人都是Android用户）。



* * *





### 那为嘛手机制造商和运营商想锁住Bootloader？这有神马意义？


简而言之，为了安全————在经济利益上，不论是对于你的运营商而言还是对于最终用户而言。

每当我们讨论有锁的Bootloader的时候，我们大多数时候，指的是一个disk image，它能在手机启动的时候检查手机某个重要模块并检查其签名是否符合要求。这样吧，让我们来仔细探讨这件事情。

当你摁开你的Atrix 4G、或者HTC Sensation，Bootloader首先起作用，然后移交给bootimage（这boot image存有你手机的启动文件）。Boot image读取手机的Kernel（内核），然后启动Android，然后就没有然后了。你刷机的时候，是把这个boot image刷入手机的存储器，不是RAM或者说运行内存，这就是所谓的风险。如果这一部虾米了，比如说你刷了一个错的boot image，那手机就有可能虾米（译者注：比如我刷虾米过一个G1），手机就成了砖⋯⋯不过这个风险不算很大，当然也取决于你打算改到多么底层的文件了， 也同时取决于手机的不同。

如果你的手机不幸被锁了Bootloader，那么很不幸，你只能去刷那些有官方签名文件的ROM（译者注：比如现在的Moto Milestone就是杯具机之一，所谓Milestone的刷机都是伪刷机，通过混刷实现的，和真正刷机的效果不可同日而语），而且你不能自己编译并且 刷到手机里面。对于Recovery而言，也是这样，它也会检查签名，完后你没有签名，于是不能刷定制的Recovery。这一坨话的意思归结为一点，就 是：


> 

> 
> #### 我们不能在锁了Bootloader的手机上安装自制内核或者任何启动文件
> 
> 



不过锁了Bootloader的手机，不影响咱root（译者又注：比如Milestone锁了Bootloader但是依然可以root）。 Root只是利用了系统安全的一个缺陷来向系统文件中注入相关破解文件，这样每次我们需要用到root权限的时候就可以随时取得。（译者继续注：比方说没 有root权限你就不能删系统的Loader、不能截图、不能OpenVPN）。
继续讨论安全的部分，如果你手机上的软件都是运营商和手机厂商给你安装的，那么基本上手机是没有安全隐患的（山寨厂的手机除外），除此之外，厂商还 会定期推出补丁包供你升级。当然这个修补漏洞的过程基本说是无穷尽的，通过锁Bootloader的方式，制造商可以尽可能的掌控并修补你手机上的安全漏 洞。不过你还记得不，一开始我们就提到过，并不是每个Android用户，都能读到这篇文章，也并不是每个人都知道厂商发布的那些升级包是做什么用的。卖 你手机的厂家可是不只会为你一个人提供升级服务的，人家会给n个相同型号的手机提供升级包。

于是通过篡改手机里的某些文件，我们可以破坏运营商的利益。你比如，通过搞起PRL，可以让你处于Virgin合约下的Optimus V手机使用Verizon的3G信号漫游，然后烧的账单却是Sprint来埋。（译者注：Virgin Mobile、Verizon和Sprint都是米国的运营商），或者开启HTC Spire的HSPA+功能，绕过T-Mobile的数据流量限制，未授权即可使用无线网络分享、或者篡改时间片循环时间、删掉Microsoft和运营 商达成合约在手机上使用的默认的Bing搜索引擎。这些运营商的策略在我们看来完全的不合理，不过你做了上面任何一条，都会极大地损害他们的利益。

于是，他们就得想办法阻止这件事儿的发生啊。



* * *





### 但是Thunderbolt锁了Bootloader不是么，那为毛这货还能有自制ROM和CyanogenMod呢？


嗯，没错，Thunderbol是锁了Bootloader，而且也确实有自制ROM和CyanogenMod。破解Thunderbolt的开发 者用了点小技巧+一些运气才成功的。他们动用了一个新版的Bootloader，这货他们可以刷，于是就用这个作为漏洞破进系统然后刷了 Recovery，从而是机器能够刷未经签名的镜像。灰常需要技巧、灰常的幸运，于是我们没理由总盼这种天上掉馅儿饼的事情。



* * *





### 够了，别白话了，我知道解锁了的Bootloader很华丽，那跟我有毛关系？


有N多关系。首先拿到一个已经解锁Bootloader的手机，你几乎能做任何事情。

Droid X的开发者是一群极其牛13的家伙们，因为他们不能刷ClockworkMod，装在自制镜像或者内核，于是他们就走了另外一条简单粗暴的路线。不过尽管 Droid X如此，他们还是搞出来点儿名堂的。于是同样的事情也会发生在同样锁了Bootloader的Evo 3D身上。与此形成对比的是，Nexus S 4G刚刚上市就被root，然后内核也被重新编译、自定Recovery也都做好了，这些几乎都在一天之内完成，全都因为Nexus S 4G是没有锁Bootloader的机器。

我们不知道HTC打算怎么处理这个解锁Bootloader的策略，不过大家都猜想很可能类似Sony Ericsson的方式——先发锁上Bootloader的手机，然后再提供给愿意解锁的用户解锁的方法。他们可能会让这类解锁的设备跳出和运营商的合 同，不过这一切都只是我们YY出来的结果，不过我确定HTC不久后会给出答复的。

当你拿到一个解锁Bootloader的手机后，对于这个手机的挖掘步骤就跟事先录制好的宏一样：先是会有root、紧接着有自定制ROM、然后就 是从其他ROM或者设备里面移植新特性过来。这也是为毛那么多人喜欢Android的原因所在。总而言之，解锁的Bootloader，意味着自定义内核 ——你可以超频手机、开启USB Host功能以及一大堆锁Bootloader的设备没法实现的功能。当然，也意味着，特别是HTC的设备可以用上MIUI和CyanogenMod。

和你一样，大家都很乐于见到自己的手机有一堆一堆的自定ROM出现。如果这正是你喜欢的用手机方式，那么可以去找一台ROM支持最多的手机 happy去，如果不喜欢折腾，那么继续用着原厂ROM也是一种很稳健的选择。不管如何，我们希望这篇文章能够解答你关于Bootloader的疑问

Via [ss1271的奋斗](http://www.yeyaxi.com/2011/06/bootloaders-more-than-you-ever-wanted-to-know/) ，译自 [Android Central](http://www.androidcentral.com/bootloaders-all-you-ever-wanted-know)

[Source [Bootloaders: More than you ever wanted to know](http://www.androidcentral.com/bootloaders-all-you-ever-wanted-know)]



