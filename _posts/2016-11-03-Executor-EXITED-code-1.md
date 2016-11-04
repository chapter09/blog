---
layout: post
title: "Excutor EXITED with code 1"
description: ""
categories: 
tags: []
---

I run the SparkPi with `sbt` incremental compilation on, and result in the follow errors:

```bash
16/11/03 13:57:01 INFO AppClient$ClientEndpoint: Executor updated: app-20161103135700-0001/10 is now RUNNING
16/11/03 13:57:01 INFO AppClient$ClientEndpoint: Executor updated: app-20161103135700-0001/10 is now EXITED (Command exited with code 1)
16/11/03 13:57:01 INFO SparkDeploySchedulerBackend: Executor app-20161103135700-0001/10 removed: Command exited with code 1
16/11/03 13:57:01 INFO SparkDeploySchedulerBackend: Asked to remove non-existent executor 10
16/11/03 13:57:01 INFO AppClient$ClientEndpoint: Executor added: app-20161103135700-0001/11 on worker-20161103135455-10.2.3.5-42428 (10.2.3.5:42428) with 4 cores
16/11/03 13:57:01 INFO SparkDeploySchedulerBackend: Granted executor ID app-20161103135700-0001/11 on hostPort 10.2.3.5:42428 with 4 cores, 4.0 GB RAM
16/11/03 13:57:01 INFO AppClient$ClientEndpoint: Executor updated: app-20161103135700-0001/11 is now RUNNING
16/11/03 13:57:01 INFO AppClient$ClientEndpoint: Executor updated: app-20161103135700-0001/11 is now EXITED (Command exited with code 1)
16/11/03 13:57:01 INFO SparkDeploySchedulerBackend: Executor app-20161103135700-0001/11 removed: Command exited with code 1
16/11/03 13:57:01 INFO SparkDeploySchedulerBackend: Asked to remove non-existent executor 11
16/11/03 13:57:01 INFO AppClient$ClientEndpoint: Executor added: app-20161103135700-0001/12 on worker-20161103135455-10.2.3.5-42428 (10.2.3.5:42428) with 4 cores
16/11/03 13:57:01 INFO SparkDeploySchedulerBackend: Granted executor ID app-20161103135700-0001/12 on hostPort 10.2.3.5:42428 with 4 cores, 4.0 GB RAM
16/11/03 13:57:01 INFO AppClient$ClientEndpoint: Executor updated: app-20161103135700-0001/12 is now RUNNING
16/11/03 13:57:01 INFO AppClient$ClientEndpoint: Executor updated: app-20161103135700-0001/12 is now EXITED (Command exited with code 1)
```

It is weird because all failed executors are from `10.2.3.5`, which is the Spark master node, also the node I am doing incremental compilation. 

Then I am wondering maybe it’s the resource shortage that leads to the executor failures. Then I check the available memory with incremental compilation on, which is around 2 GB, while I assigned 4 GB to an executor. I then assigned only 2GB to the executor, and launch SparkPi. It succeed ! I have located the problem. 