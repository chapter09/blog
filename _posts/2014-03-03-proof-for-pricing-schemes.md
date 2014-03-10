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
\end{equation} (1)

which is a concave function of $d$. On the other hand, taking derivation on both sides of (9) with respect to $p$, we know

\begin{equation}\label{eq11}
    f'(d)d' = d + pd'             
\end{equation}

That is $d'=\frac{{{d^2}}}{{f'(d)d - f(d)}}$

Since $f(0)=0$

$$\begin{array}{l}
f'(d)d - f(d) = f'(d)d - f(d) + f(0)\\
\quad \quad \quad \quad = [f'(d) - f'({d^*})]d
\end{array}$$

for some $${d^*} \in (0,d)$$. For $f(d)$ is a concave function, there must be $$f'(d)<f'(d^*)$$, so that $d'<0$ and $d$ is a decreasing function of $p$. 􏰗


###Lemma 2: 
**The demand quantity $d$ is a concave function of price of each unit resource, if and only if $f''(d)d'>2$. \footnote{For brevity, the proof of Lemma 1 and Lemma 2 is given in**

__Proof__: Take derivation on both sides of (11) with respect to $p$, we can get

<!--$$f''(d){(d')^2} + f'(d)d'' = 2d' + pd''$$ That is to say

$$d'' = \frac{{2d' - f''(d){{(d')}^2}}}{{f'(d) - p}}$$ From (11), we know

$$f'(d) - p = d/d'$$ so that

$$\begin{array}{l}
d'' = [2{(d')^2} - f''(d){(d')^3}]/d\\
\quad  = [2 - f''(d)d']{(d')^2}/d
\end{array}$$ 

Since $d>0$ and $d'<0$,  $d''<0$ iff $f''(d)d' >2$. \hfill $\blacksquare$-->
