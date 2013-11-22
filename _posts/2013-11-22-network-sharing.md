---
layout: post
title: "Network Sharing"
description: ""
categories:
- Network 
tags: []
---
{% include JB/setup %}


####Towards Predictable Datacenter Networks
----

Problem:

>1. DCN中网络的performance变化很大，难以保障tenant运行程序正常运行。从而对tenant和provider都造成损失。

Contribution:

>1. 提出描述tenant需求的两个模型
>2. 通过模型，保证tenant的带宽需求和provider的利益。（如，缩短job完成时间）

Method:

>1. 提出两个需求模型：virtual cluster和virtual oversubscribed cluster。通过&#60;N,B&#62;和&#60;N,B,S,O&#62;，tenant能够很好地表达自己的需求，同时provider也能更好的实施这些需求(比如，VM allocation)

>2. 针对两个模型，设置参数，并提出了Allocation算法。在VC中计算每条链路的带宽需求，结合Rack上的空闲server数来完成&#60;N,B&#62;的request。同样地，&#60;N,B,S,O&#62;的VOC模型的实施也要结合物理拓扑做类似的计算。

>3. job分为计算时间，和完成时间。Network的lag导致需要等待flow的到来。完成时间是一个重要的metric

>4. 一系列实验，对完成时间，带宽request，利润与负载等在不同模型和baseline的比较数据。

Related:

1. 具体路由的实现。在DCN中物理链路的拓扑会比较复杂，因此如果路由完成request要求的VC拓扑需要使用ECMP或者VLB来做流量路由。

2. Failure的处理。论文中说目前物理链路或者switch除了问题。providers不负责而Tenant需要为此付费：

> With today’s setup, providers are not held responsible for physical failures and tenants end up paying for them
