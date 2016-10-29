---
layout: post
title: "Spark Applicaiton failed and solution"
description: ""
categories: 
tags: []
---

###环境

* Spark 1.0.0 []Standalone Mode]
* Hadoop 2.2.0
* Debian 6
* jdk1.7.0_25


###问题一

我将example.bagel下面的`WikipediaPageRank`拿出来，根据Spark Quick Start的Standalone Applications，把WikipediaPageRank独立成IntelliJ的project。

通过`sbt package`然后sbt console下`run xxx parameters`来执行pagerank算法。

但是run的结果是遇到errors：

	14/06/12 20:19:51 WARN AppClient$ClientActor: Connection to akka.tcp://sparkMaster@sing12:7077 failed;
	
这个错误我首先判断可能是版本的问题，cluster里面的机器见Spark版本不一样。然后我重新在Master上build，然后通过`rsync`进行同步。

然后看到网上有帖子说`MASTER`应该写`hostname` 而不是`IP`。我把配置文件和Application source Code里统一用`hostname`。_// 这个问题我得读读源码才能说得清楚_

报错为：
	
	14/06/13 22:09:43 INFO DAGScheduler: Submitting 104 missing tasks from Stage 0 (MappedRDD[1] at textFile at WikipediaPageRank.scala:59)
	14/06/13 22:09:43 INFO TaskSchedulerImpl: Adding task set 0.0 with 104 tasks
	14/06/13 22:09:58 WARN TaskSchedulerImpl: Initial job has not accepted any resources; check your cluster UI to ensure that workers are registered and have sufficient memory
	
应该改为：

	sparkConf.setMaster("spark://hostname:7077")


随后再run出现`ClassNotDefined`的错误。这个实际上是因为没有指定Jar包，应该设置SparkContext:

	sc.addJar("target/scala-2.10/wikipagerank_2.10-1.0.jar")


###问题二
另一个问题比较naive了。忘记在Application的build.bst里面加hadoop相关的依赖了。

我是用Scala的，所以应该是加上：

	libraryDependencies += "org.apache.hadoop" % "hadoop-client" % "2.2.0"
