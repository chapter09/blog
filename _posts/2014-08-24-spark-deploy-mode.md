---
layout: post
title: "Spark Deploy Mode"
description: ""
categories: 
tags: []
---
{% include JB/setup %}

###1. Spark *init* order

For simplicity, we set up two roles:

* **Spark Admin**: who maintain the whole Spark cluster
* **Spark User**: who submit applications, *e.g.* KMeans, PageRank

Firstly, the Admin will launch the Spark `Master` and `Worker` via scripts (*SPARK_HOME/start-all.sh, start-master.sh, start-workers.sh etc.*). The initialization logs are recorded in the directory `SPARK_HOME/logs/`. Additionally, when the user submits applications to the Spark cluster, logs of master/workers objects will still be appended to log files in the directory. 

###2. Run an application
* run