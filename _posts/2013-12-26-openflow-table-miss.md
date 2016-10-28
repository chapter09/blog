---
layout: post
title: "OpenFlow Table Miss"
description: ""
categories: 
tags: []
---
{% include JB/setup %}

from openflow-spec-v1.3.3

每一个flow table都需要设置一个table-miss flow entry。这个entry规定了如果处理没有被match的packet。比如drop掉，发给controller或者查下一级的flow table.

目前物理open flow switch 上flow table的size仍是有限的。Pica8 的1Gbps switch是4000个entry，10Gbps是2000个。

通常情况下，如果table满了，一个unmatch的packet会被转给controller，controller则写入新的flow，但是这个时候会造成一个OFPMFC_ALL_TABLES_FULL的ErrorIn到controller。
