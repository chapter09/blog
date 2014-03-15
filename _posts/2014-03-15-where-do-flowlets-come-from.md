---
layout: post
title: "Where Do Flowlets Come From?"
description: ""
categories: 
tags: []
---
{% include JB/setup %}

>[1] S. Sinha, S. Kandula, and D. Katabi, “Harnessing TCPs burstiness using flowlet switching,” presented at the Proceedings of the 4th …, 2004.

The origins of flowlets are not limited to very short flows, flows that are just starting with a small window of one or two packets, or flows that are suffering timeouts. These flowlet sources are not enough to make flowlet splitting as effective as IV shows because most of the bytes are in the long TCP flows [32]. In fact, the main origin of flowlets is the burstiness of TCP at RTT and sub-RTT scales. Prior work has shown that the TCP sender **tends to send a whole congestion window in one burst or a few clustered bursts and then wait idle for the rest of its RTT.** This behavior is caused by ack compression, slow-start, and other factors [15, 30, 33]. This burstiness enables a FLARE router to consider a long TCP flow as a concatenation of short flowlets separated by idle periods, which is necessary for the success of flowlet-based splitting.
Figure 7 supports this argument. It was computed using the peering trace for Æ =60 ms. The figure plots the time between ar- rivals of two consecutive flowlets from the same flow normalized by the RTT of the flow (RTT is computed using the MYSTERY TCP analyzer [18]). **The graph shows that the vast majority of flowlets are separated by less than an RTT, indicating that a flowlet is usually a congestion window or a portion of it.**
Flowlets come from one burst or a few clustered bursts which make up __one congestion windows__ and then wait idle for the rest of its RTT