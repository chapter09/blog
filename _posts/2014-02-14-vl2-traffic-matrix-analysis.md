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
Given the timeseries of TMs, we find clusters of similar TMs using a technique due to Zhang et al.[22]. In short, the technique recursively collapses the traffic matrices that are most similar to each other into a cluster, where the distance (i.e., similarity) reflects how much traffic needs to be shuffled to make one TM look like the other. We then choose a representative TM for each cluster, such that any routing that can deal with the representative TM performs no worse on every TM in the cluster. Using a single representative TM per cluster yields a fitting error (quantified by the distances be- tween each representative TMs and the actual TMs they represent), which will decrease as the number of clusters increases. Finally, if there is a knee point (i.e., a small number of clusters that reduces the fitting error considerably), the resulting set of clusters and their representative TMs at that knee corresponds to a succinct number of distinct traffic matrices that summarize all TMs in the set.

这一段看了有一会。现在应该算看明白了: 首先在一个1500 servers的cluster里面，记录了相当一天意义的Traffic Matrix的数据。

组成的{$TM1_$, $TM_2$, ..., $TM_m$ }> Zhang, Y., & Ge, Z. (2005). Finding critical traffic matrices (pp. 188–197). Presented at the Dependable Systems and Networks, 2005. DSN 2005. Proceedings. International Conference on, IEEE. doi:10.1109/DSN.2005.51