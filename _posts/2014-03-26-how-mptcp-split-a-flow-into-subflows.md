---
layout: post
title: "How MPTCP split a flow into subflows?"
description: ""
categories: 
tags: []
---
{% include JB/setup %}

> [1]	C. Raiciu, S. Barre, C. Pluntke, A. Greenhalgh, D. Wischik, and M. Handley, “Improving datacenter performance and robustness with multipath TCP,” ACM SIGCOMM Computer Communication Review, vol. 41, no. 4, pp. 266–277, Aug. 2011.

> [2]	“MultiPath TCP: From Theory to Practice,” pp. 1–29, May 2011.

> [3]	“TCP Extensions for Multipath Operation with Multiple Addresses,” pp. 1–64, Feb. 2014.

MPTCP defines DATA_ACK on connection level, while the ACK on subflow level works as SACK. According to the RFC 6824: __a sender is allowed to send data segments with data-level sequence numbers between (DATA_ACK, DATA_ACK + receive_window).  Each of these segments will be mapped onto sub flows, as long as subflow sequence numbers are in the allowed windows for those subflows.__ 

A connection shares a unified receive window among its subflows, and DATA_ACK will drive this receive window to update. When the connection distributes the TCP segments whose sequence numbers (DSN, Data Sequence Number) are in the range of receive window, the segments will be put into different subflows. However, there is still a `receive window` on subflow level for SSN(Subflow Sequence Number). Only the segments in 