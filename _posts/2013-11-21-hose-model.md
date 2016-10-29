---
layout: post
title: "Hose Model"
description: ""
categories: 
- Network
tags: []
---


读了好些关于带宽分配的paper，多次碰到Hose Model这一概念。比如一下的论文：

>[1] L. Popa, P. Yalagandula, S. Banerjee, and J. C. Mogul, “ElasticSwitch: Practical Work-Conserving Bandwidth Guarantees for Cloud Computing,” 2013.

>[2] H. Ballani, K. Jang, T. Karagiannis, and C. Kim, “Chatty tenants and the cloud network sharing problem,” presented at the Proceedings of the 10th …, 2013.

>[3] L. Popa, G. Kumar, M. Chowdhury, A. Krishnamurthy, S. Ratnasamy, and I. Stoica, “FairCloud: sharing the network in cloud computing,” presented at the SIGCOMM '12: Proceedings of the ACM SIGCOMM 2012 conference on Applications, technologies, architectures, and protocols for computer communication, New York, New York, USA, 2012, pp. 187–198.

>[4] H. Ballani, P. Costa, T. Karagiannis, and A. Rowstron, “Towards predictable datacenter networks,” presented at the SIGCOMM '11: Proceedings of the ACM SIGCOMM 2011 conference, New York, New York, USA, 2011, p. 242.		

>[5] P. Soares, J. Santos, N. Tolia, D. Guedes, and Y. Turner, “Gatekeeper: Distributed Rate Control for Virtualized Datacenters,” 2010.

实际上这些论文里的Hose Model都是来源于以下SIGCOMM1999的一篇论文

>N. G. Duffield, P. Goyal, A. Greenberg, and P. Mishra, “A flexible model for resource management in virtual private networks,” ACM SIGCOMM …, 1999.

![figure_1](http://f.hiphotos.bdimg.com/album/s%3D550%3Bq%3D90%3Bc%3Dxiangce%2C100%2C100/sign=2d2c3e2eb8a1cd1101b672258929b9c1/d000baa1cd11728b92e0563ccafcc3cec2fd2cdb.jpg?referer=1d653e6daf4bd1135dda83028cb9&x=.jpg)Figure-1

A flexible model for resource management in virtual private networks 其实讲的是VPN下的资源分配模型。

最早VPN用是pipeline的方式，这样导致的问题是A，B，C，D四个客户相互连接的pipeline无法复用和灵活调整，客户需要精确地根据traffic matrix分配，因为每条pipeline都限制好了。比如A-D的线路，A-B的线路对于A来说，如果A-D的带宽暂时没用，A-B就没法用上A-D的空闲带宽。

> The primary disadvantage of this approach is that it requires the customer to have precise knowledge of the traffic matrix between all the VPN sites. Resources made available to a customer-pipe cannot be allocated to other traffic. 

如Figure-2：

![figure_2](http://e.hiphotos.bdimg.com/album/s%3D550%3Bq%3D90%3Bc%3Dxiangce%2C100%2C100/sign=b6e12a380b24ab18e416e13205c197f0/d1160924ab18972b3080cfd4e4cd7b899e510a72.jpg?referer=ae170c2d718da9771738b31b7611&x=.jpg)Figure-2

因此有了Figure-1，通过软管这样的逻辑接口，可以让customer的出口和入口统一起来。控制软管的出口和进口，并提供性能保证。

>In the hose model, a VPN customer specifies a set of endpoints to be connected with common endpoint-to-endpoint performance guarantees. The connectivity of each endpoint to the net- work is specified by a hose

![figure_3](http://g.hiphotos.bdimg.com/album/s%3D550%3Bq%3D90%3Bc%3Dxiangce%2C100%2C100/sign=887604272a381f309a198dac993a3d35/060828381f30e924aec0aa2e4e086e061d95f74c.jpg?referer=64570fdcd5ca7bcb246cf21f3142&x=.jpg)Figure-3

因此到了目前的DCN里面，应用Hose Modle的场景就是这样的了：

![figure_4](http://f.hiphotos.bdimg.com/album/s%3D550%3Bq%3D90%3Bc%3Dxiangce%2C100%2C100/sign=382f01f8932397ddd279980169b9c38a/0dd7912397dda144f02354adb0b7d0a20cf48682.jpg?referer=f246fef1b68f8c54bac4f01fe280&x=.jpg)Figure-4

每一条link上可以有多个flow，而这些flow在软管中的带宽是可以灵活调节的。另外这些link不一定是物理上的link，可能是逻辑上的link。

Hose Model于是体现在了DCN里面的链路里。

![figure_5](http://h.hiphotos.bdimg.com/album/s%3D550%3Bq%3D90%3Bc%3Dxiangce%2C100%2C100/sign=543806c834d12f2eca05ae657ff9a45f/55e736d12f2eb938b4841dedd7628535e4dd6fc7.jpg?referer=b3568377cebf6c81ae2018d8c9dd&x=.jpg)

在SIGCOMM2013的这篇论文里便应用了Hose Model。简单的说其实就是把物理上的link聚合在一起看成可以灵活划分bandwidth的软管。实际上一个软管之下的物理链路是复杂的，但是通过软管能在逻辑的高度上灵活划分带宽。

这也就是为什么在SIGCOMM2011 的Towards Predictable Datacenter Networks中说到：

> The hose model is akin to the virtual cluster abstraction

Hose Model或者Virtual Cluster都是逻辑的抽象。

>L. Popa, P. Yalagandula, S. Banerjee, and J. C. Mogul, “ElasticSwitch: Practical Work-Conserving Bandwidth Guarantees for Cloud Computing,” 2013.