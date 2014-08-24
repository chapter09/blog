---
layout: post
title: "Spark Deploy Mode: how Driver node is launched?"
description: ""
categories: 
tags: []
---
{% include JB/setup %}

###1. Spark *init* order

For simplicity, we set up two roles:

* **Spark Admin**: who maintain the whole Spark cluster
* **Spark User**: who submit applications, *e.g.* KMeans, PageRank

Firstly, the Admin will launch the Spark `Master` and `Worker` via scripts (*SPARK_HOME/start-all.sh, start-master.sh, start-workers.sh etc.*). The initialization logs are recorded in the directory `SPARK_HOME/logs/`. Additionally, when the user submits applications to the Spark cluster, logs of master/workers objects will still be appended to log files in the directory. More info please refer [here](https://github.com/JerryLead/SparkInternals/blob/master/markdown/1-Overview.md).

Besides, there are various parameters to initialize the Spark cluster, *e.g.* deploy on YARN, Mesos, Standalone. In general, the deloy mode is setup in system *env*. 

###2. Run an application
* As far as I know, there are two approaches to submit an application:
	* sbt run (application configurations including CPU cores, memory, Master URL *etc.* have been set up in application source code.)
	* spark-submit script. We mainly focus on this method in following paragraphs.

When using `spark-submit` to submit applications, a parameter `deploy-mode` is used to set up deployMode in `SparkSubmit.scala`. 

In `SparkSubmit.scala`:

    val deployOnCluster = Option(args.deployMode).getOrElse("client") == "cluster"
    
Thus, in `StandAlone Mode` when `deployMode` is `client`, the `spark-submit` will run as the `Driver`. 

While if the `deployMode` is 'cluster', for `StandAlone Mode`, the `spark-submit` will launch `org.apache.spark.deploy.Client`. In `Client` a request for deploying the `Driver` will be sent to the Master Actor. 
 