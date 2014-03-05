---
layout: post
title: "Proof for Pricing Schemes in DCN with Game Theoretic Approach"
description: ""
categories: 
tags: []
---
{% include JB/setup %}

###Lemma 1: 
**When the operator announces a higher price, there will be less demand sent into the network.**

__Proof__: From $U(d, p)=0$, we know 
\begin{equation}\label{eq9}
    f(d) = pd
\end{equation}
substitute $pd$ in (2) with (9), there is
\begin{equation}\label{eq10}
    V(d, p)=f(d)-vg(d)
\end{equation}

which is a concave function of $d$. On the other hand, taking derivation on both sides of (9) with respect to $p$, we know

\begin{equation}\label{eq11}
    f'(d)d' = d + pd'             
\end{equation}

That is $d' = \frac{{{d^2}}}{{f'(d)d - f(d)}}$

Since $f(0)=0$

$$\begin{array}{l}
f'(d)d - f(d) = f'(d)d - f(d) + f(0)\\
\quad = [f'(d) - f'({d^*})]d
\end{array}$$

for some $${d^*} \in (0,d)$$. For $f(d)$ is a concave function, there must be $$f'(d)<f'(d^*)$$, so that $d'<0$ and $d$ is a decreasing function of $p$. 􏰗