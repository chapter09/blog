---
layout: post
title: "Apache Spark incremental compilation tips"
description: ""
categories: 
tags: []
---

Building Apache Spark from source code is nevey a time-saving job. Either `mvn` or `sbt` consumes quite a lot time in compilation. To this end, __incremental compilation__ or __continuous compilation__ will be the saver.

It is recommended in Spark official documents that `sbt` is more suitable for day-to-day build:

> But SBT is supported for day-to-day development since it can provide much faster iterative compilation. 

The first step is to create a fat jar, which includes all of Spark's dependencies:

```bash
./build/sbt -Pyarn -Phadoop-2.7 -Dhadoop.version=2.7.3 -Dscala-2.11 -Phive -Phive-thriftserver -DskipTests assembly 
```

We will have a large jar file like this:

```bash
$ ls -hl assembly/target/scala-2.11
total 184M
-rw-rw-r-- 1 ubuntu ubuntu 184M Oct 31 15:28 spark-assembly-1.6.1-hadoop2.7.3.jar
```

Then create a seperated jar package for Spark itself, and incremental compilation will be conducted on this jar.

```bash
./build/sbt -Pyarn -Phadoop-2.7 -Dhadoop.version=2.7.3 -Dscala-2.11 -Phive -Phive-thriftserver -DskipTests package
```

Then we will have:

```bash
~/spark-1.6.1
❯ ls -hl assembly/target/scala-2.11
total 184M
-rw-rw-r-- 1 ubuntu ubuntu 184M Oct 31 15:28 spark-assembly-1.6.1-hadoop2.7.3.jar
-rw-rw-r-- 1 ubuntu ubuntu  281 Oct 31 15:42 spark-assembly_2.11-1.6.1.jar
```

Or in an interactive way:

```bash
$ ./build/sbt -Pyarn -Phadoop-2.7 -Dhadoop.version=2.7.3 -Dscala-2.11 -Phive -Phive-thriftserver -DskipTests
> assembly 
....
> package
```