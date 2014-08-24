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




