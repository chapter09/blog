---
layout: post
title: "VL2 Traffic Matrix Analysis"
description: ""
categories: 
- Network
tags: []
---
{% include JB/setup %}

> Greenberg, A., Hamilton, J. R., Jain, N., Kandula, S., Kim, C., Lahiri, P., et al. (2009). VL2: a scalable and flexible data center network (p. 51). Presented at the SIGCOMM '09: Proceedings of the ACM SIGCOMM 2009 conference on Data communication, New York, New York, USA:  ACM  Request Permissions. doi:10.1145/1592568.1592576


For computational tractability, we compute the ToR-to-ToR TM — the entry TM(t)i,j is the number of bytes sent from servers in ToR i to servers in ToR j during the 100 s beginning at time t. We compute one TM for every 100 s interval, and servers outside the cluster are treated as belonging to a single “ToR”.
Given the timeseries of TMs, we find clusters of similar TMs using a technique due to Zhang et al.[*]. In short, the technique recursively collapses the traffic matrices that are most similar to each other into a cluster, where the distance (i.e., similarity) reflects how much traffic needs to be shuffled to make one TM look like the other. We then choose a representative TM for each cluster, such that any routing that can deal with the representative TM performs no worse on every TM in the cluster. Using a single representative TM per cluster yields a fitting error (quantified by the distances be- tween each representative TMs and the actual TMs they represent), which will decrease as the number of clusters increases. Finally, if there is a knee point (i.e., a small number of clusters that reduces the fitting error considerably), the resulting set of clusters and their representative TMs at that knee corresponds to a succinct number of distinct traffic matrices that summarize all TMs in the set.

这一段看了有一会。现在应该算看明白了: 首先在一个1500 servers的cluster里面，记录了相当一天意义的Traffic Matrix的数据。

组成的{$TM\_1$ , $TM\_2$, ..., $TM\_m$}, distance实际上就是$TM\_i$矢量间的距离，代表着$TM$各个分量间的差。

通过合并这些$TM\_i$, 组成一个`cluster`，里面包含相似的$TM$。根据`fitting error`的定义，如果`cluster`数量分的越多，即$TM$分得越细，自然一个`cluster`里面的`fitting error`越小。

论文的数据是864个$TM$，当分成50~60个`cluster`的时候，fitting error仍然有60%。说明整个cluster的$TM$变化很大，很难用一个统一的routing规则或者$TM$去满足不同时间段的demand.

论文里结论是:`This indicates that the variability in datacenter traffic is not amenable to concise summarization and hence engineering routes for just a few traffic matrices is unlikely to work well for the traffic encountered in practice.`

![](http://d.hiphotos.bdimg.com/album/s%3D550%3Bq%3D90%3Bc%3Dxiangce%2C100%2C100/sign=8c496058ba99a9013f355b332dae7b46/e824b899a9014c08fd7f94e9087b02087bf4f432.jpg?referer=e8b2833af01fbe094549f72434d0&x=.jpg)

图中纵轴是0-39个编号的`Cluster`，这些其实是从864个$TM$中选出来`representative TM`，在800多秒的区间内，每个`+`其实就是一个$TM$，这样的分布，足以可见DCN里面的Network没有固定pattern，因此更无法predict了。
* Zhang, Y., & Ge, Z. (2005). Finding critical traffic matrices (pp. 188–197). Presented at the Dependable Systems and Networks, 2005. DSN 2005. Proceedings. International Conference on, IEEE. doi:10.1109/DSN.2005.51