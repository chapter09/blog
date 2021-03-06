---
layout: post
title: "Proof for Pricing Schemes in DCN with Game Theoretic Approach"
description: ""
categories: 
tags: []
---
{% include JB/setup %}

We assume that the customer earns $f(d)$ from $d$ units of demands, where $f(d)$ is an increasing and concave function of $d$. Let $g(d)$ denote the energy consumption of operator if it gives $d$ units of resource to the customer and $v$ denotes the cost which operator should pay for each unit of energy. Usually, $g(d)$ is a convex function [Enhanced intel speed step technology for the intel pentium m processor. Intel White Paper 201170-001, 2004.].

Now, the customer's utility is:

$$       U(d,p) = f(d) - pd    \quad\quad\quad(1)$$

if the operator charges a fee $p$ for each unit of demands. And the operator's utility is:

$$       V(d,p) = pd - vg(d)   \quad\quad\quad(2)$$

###Lemma 1: 
**When the operator announces a higher price, there will be less demand sent into the network.**

__Proof__: From $U(d, p)=0$, we know 

$$    f(d) = pd \quad\quad\quad\quad\quad(3)$$

substitute $pd$ in (2) with (3), there is

$$    V(d, p)=f(d)-vg(d) \quad\quad\quad(4)$$

which is a concave function of $d$. On the other hand, taking derivation on both sides of (3) with respect to $p$, we know

$$    f'(d)d' = d + pd'      \quad\quad\quad\quad(5)       $$

That is 

$$d'=\frac{d^2}{f'(d)d - f(d)}$$

Since $f(0)=0$

$$\begin{array}{l}
f'(d)d - f(d) = f'(d)d - f(d) + f(0)\\
\quad \quad \quad \quad = [f'(d) - f'({d^*})]d
\end{array}$$

for some $${d^*} \in (0,d)$$. For $f(d)$ is a concave function, there must be $f'(d)<f'(d^*)$, so that $d'<0$ and $d$ is a decreasing function of $p$. █


###Lemma 2: 
**The demand quantity $d$ is a concave function of price of each unit resource, if and only if $f''(d)d'>2$.**

__Proof__: Take derivation on both sides of (5) with respect to $p$, we can get

$$f''(d){(d')^2} + f'(d)d'' = 2d' + pd''$$ 

That is to say

$$d'' = \frac{2d' - f''(d){(d')}^2}{f'(d) - p}$$ 

From (5), we know

$$f'(d) - p = d/d'$$ 

so that

$$ d'' = [2{(d')^2} - f''(d){(d')^3}]/d\\
\quad  = [2 - f''(d)d']{(d')^2}/d $$

Since $d>0$ and $d'<0$,  ${d''}<0$ iff ${f''}(d){d'} >2$. █



###Proposition 1: 
**If the price announced in Subnetwork 1 is $p_1$, the price announced in Subnetwork 2 $p_2$ should be in $[u(s_1), p\_{max}]$, where $s_1$ is the strongest Qos requirement in Subnetwork 1 which can be calculated by solving $D\int\_{s_1}^{u^{-1}(p_1)}{q(x)dx}={s_1}{C_1}$, and $p_{max}$ is determined by the strongest Qos requirement may exist in the network.**

__Proof__: If $p_2 \in (p_1, u(s_1))$, since all the demands whose Qos requirement is in $(u^{-1}(p_2), s_1)$ are sent into Subnetwork 1, the weakest QoS requirement in Subnetwork 2 will be $s_1$, and the strongest QoS requirement in Subnetwork 2 is determined by solving 
$D\int_{s_2}^{s_1} {q(x)dx} = {s_2}{C_2}$
The operator can charge

$${p_1}D\int_{s_1}^{u^{ - 1}(p_1)}{q(x)dx + {p_2}D\int_{s_2}^{s_1}{q(x)dx}}$$

for its resource. If the operator simply announces the resource price as $u(s_1)$ in Subnetwork 2, the demand sent into Subnetwork 2 will not change (i.e. the energy consumption does not change) but it will charge

$${p_1}D\int_{s_1}^{u^{ - 1}(p_1)}{q(x)dx + u(s_1)D\int_{s_2}^{s_1}{q(x)dx}}$$

for requested resource, which is larger than it announce a price in $(u^{-1}(p_2), s_1)$. █         




