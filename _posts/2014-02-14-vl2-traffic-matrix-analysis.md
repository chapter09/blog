---
layout: post
title: "VL2 Traffic Matrix Analysis"
description: ""
categories: 
tags: []
---
{% include JB/setup %}

> Greenberg, A., Hamilton, J. R., Jain, N., Kandula, S., Kim, C., Lahiri, P., et al. (2009). VL2: a scalable and flexible data center network (p. 51). Presented at the SIGCOMM '09: Proceedings of the ACM SIGCOMM 2009 conference on Data communication, New York, New York, USA:  ACM  Request Permissions. doi:10.1145/1592568.1592576


For computational tractability, we compute the ToR-to-ToR TM — the entry TM(t)i,j is the number of bytes sent from servers in ToR i to servers in ToR j during the 100 s beginning at time t. We compute one TM for every 100 s interval, and servers outside the cluster are treated as belonging to a single “ToR”.


这一段看了有一会。现在应该算看明白了: 首先在一个1500 servers的cluster里面，记录了相当一天意义的Traffic Matrix的数据。

组成的{$$TM_1$$, $$TM_2$$, ..., $$TM_m$$ }