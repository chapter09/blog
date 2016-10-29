---
layout: post
title: "SecondNet"
description: ""
categories: 
- Network
tags: [Network]
---

> [1]	C. Guo, G. Lu, H. J. Wang, S. Yang, C. Kong, P. Sun, W. Wu, and Y. Zhang, “SecondNet: a data center network virtualization architecture with bandwidth guarantees,” presented at the Co-NEXT '10: Proceedings of the 6th International COnference, New York, New York, USA, 2010, p. 1.

Problem:



Method:

1. VDC + VDC allocation + VDC extend

2. type-0 + type-1 + best-effort service
	* type-0 bandwidth guarantee
	* type-1 ingress/egress last and/or first hop guarantee 	

3. port-switch + source-routing + (static switch)


Design:

1. VDC manager -> build a signaling channel (STP)生成树，邻居相互通知各自的层数 etc.

Evaluation:



Conclusion:



