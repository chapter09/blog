---
layout: post
title: "Where Do Flowlets Come From?"
description: ""
categories: 
tags: []
---
{% include JB/setup %}

>[1] S. Sinha, S. Kandula, and D. Katabi, “Harnessing TCPs burstiness using flowlet switching,” presented at the Proceedings of the 4th …, 2004.

The origins of flowlets are not limited to very short flows, flows that are just starting with a small window of one or two packets, or flows that are suffering timeouts. These flowlet sources are not enough to make flowlet splitting as effective as IV shows because most of the bytes are in the long TCP flows [32]. In fact, the main origin of flowlets is the burstiness of TCP at RTT and sub-RTT scales. Prior work has shown that the TCP sender **tends to send a whole congestion window in one burst or a few clustered bursts and then wait idle for the rest of its RTT.** This behavior is caused by ack compression, slow-start, and other factors [15, 30, 33]. __This burstiness enables a FLARE router to consider a long TCP flow as a concatenation of short flowlets__ separated by idle periods, which is necessary for the success of flowlet-based splitting.
Figure 7 supports this argument. It was computed using the peering trace for Æ =60 ms. The figure plots the time between ar- rivals of two consecutive flowlets from the same flow normalized by the RTT of the flow (RTT is computed using the MYSTERY TCP analyzer [18]). **The graph shows that the vast majority of flowlets are separated by less than an RTT, indicating that a flowlet is usually a congestion window or a portion of it.**
Flowlets come from one burst or a few clustered bursts which make up __one congestion windows__ and then wait idle for the rest of its RTT
即大部分flowlets来自于TCP burst. TCP的这种burstiness属性可以让FLARE把一个long flow当成多个Flowlet的集合。
$cwnd \le BDP = RTT * bandwidth$，而68%的flow let之间间隔小于或等于1个RTT，因此可以认为这些flow let是一个congestion window或者是cwnd的一部分。
