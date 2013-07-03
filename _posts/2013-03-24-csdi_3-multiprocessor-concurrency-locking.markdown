---
author: wanghao
comments: true
date: 2013-03-24 06:12:46+00:00
layout: post
slug: csdi_3-multiprocessor-concurrency-locking
title: 'CSDI_3: Multiprocessor, Concurrency & Locking '
wordpress_id: 981
categories:
- Tech
---

Non-scalable locks are dangerous, Silas Boyd-Wickizer, M. Frans Kaashoek, Robert Morris, and Nickolai Zeldovich. In the Proceedings of the Linux Symposium, Ottawa, Canada, July 2012.

Questions:



	
  1. _Why does the performance of ticket locks collapse with a small number of cores? (It's helpful to know more about the cache coherence protocol in order to understand the cause more clearly, a recommended reading is Paul McKenney's paper Memory Barriers: a Hardware View for Software Hackers)_


The collapse of the ticket lock only occurs for small number of cores, which can be understood by considering how the service rate si decays for different lengths of the serial section. For a short serial section time s, sk = 1/(s + ck/2) is stongly influenced by k, but for large s, sk is largely unaffected by k.<!-- more -->

Another way to understand this result is that, with fewer acquire and release operations, the ticket lock’s performance contributes less to the overall application throughput.



	
  1. _How to use back off to make ticket lock more scalable? Why would it work? (It would be very rewarding trying to implement the algorithm and evaluate the performance by yourself.)_


By multiplying a constant number with ticket number, there will be a relatively long period between requests from cores, and the queue will not be that contended, and probability of collapse of performance will drop.

	
  1. _Open question: Is there any problem with the benchmarks used in the paper? Why the Linux kernel does not use the scalable locks mentioned in this paper?_


The benchmarks cover only some aspects of the lock-dealing circumstance, maybe more benchmarks could be introduced, like benchmark for database transactions and HTTP server. Besides, benchmarks that do not need multi-core processing could show the exactly performance of machines in daily workload, as not only multi-task run on machines.

In my opinion, as most Linux machines do not have cores up to that number, which could result in collapse, so Linux kernel does not adopt the scalable locks. For machines with small number of cores, scalable locks in the back off way will bring in delays.
