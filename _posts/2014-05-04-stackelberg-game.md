---
layout: post
title: "Stackelberg Game"
description: ""
categories: 
tags: []
---

Stackelberg game has been widely used in studying the pricing issue in the networks. It formulates the leader-follower relationship among players. In this game, the leader first gives out its strategy and then the follower makes its decision based on the leader's strategy and other considerations. Of course, when the leader chooses its strategy, it should consider the response of follower.

Take the pricing issue between operator and customer as an example, where the operator determines its price while customer determines its demand quantity. Let $$p$$ be the price that operator announces and $$d$$ be the demand quantity sent into the network by customer. The utilities for customer and operator are denoted by $$U(p, d)$$ and $$V(p, d)$$, respectively. Given any price $$p$$, the customer determines its demand by $$d(p)=arg~max_{d\in D}~U(p, d)$$, where $$D$$ is the set of all possible $$d$$. Considering this reaction of customer, the operator chooses its price by $$p^{*}=arg~max_{p\in P}~V(p, d(p))$$, where $$P$$ is the set of all possible price can be used by operator. $$(p^{*}, d(p^{*}))$$ is called Stackelberg equilibrium and the corresponding utility of each player $$(U(p^{*}, d(p^{*}))$$, $$V(p^{*}, d(p^{*})))$$ is called the outcome of this game. We also call the sum of the operator’s and customer’s utility $$U(p, d)+ V(p, d)$$ as the total utility of the system.

We model the interaction between operator and customer(s) as a Stackelberg game. Operator and customer(s) are players in this game, and operator is the leader while customer(s) is the follower(s). The operator’s strategy is the price it announced for its resources, and the customer’s strategy is the quantity of resources it requests from data center.


