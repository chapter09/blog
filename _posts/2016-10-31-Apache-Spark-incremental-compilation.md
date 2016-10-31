---
layout: post
title: "Apache Spark incremental compilation tips"
description: ""
categories: 
tags: []
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [1. `sbt`](#1-sbt)
- [2. `mvn`](#2-mvn)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

Building Apache Spark from source code is nevey a time-saving job. Either `mvn` or `sbt` consumes quite a lot time in compilation. To this end, __incremental compilation__ or __continuous compilation__ will be the saver.

#### 1. `sbt`

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
$ ls -hl assembly/target/scala-2.11
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

Enter the incremental compilation mode:

```bash
$ ./build/sbt -Pyarn -Phadoop-2.7 -Dhadoop.version=2.7.3 -Dscala-2.11 -Phive -Phive-thriftserver -DskipTests
> ~compile
```

To launch Spark from the seperated jar, we have to set the env variable:

```bash
$ export SPARK_PREPEND_CLASSES=true
$ ./bin/start-all.sh
``` 

Please refer to [http://www.voidcn.com/blog/lovehuangjiaju/article/p-4669432.html](http://www.voidcn.com/blog/lovehuangjiaju/article/p-4669432.html)

With this env variable, the launch path of Spark is different:

```bash
# Add the launcher build dir to the classpath if requested.
if [ -n "$SPARK_PREPEND_CLASSES" ]; then
  LAUNCH_CLASSPATH="${SPARK_HOME}/launcher/target/scala-$SPARK_SCALA_VERSION/classes:$LAUNCH_CLASSPATH"
fi
```

#### 2. `mvn`

For `mvn`, the incremental compilation is similar:

```bash
$ ./build/mvn -Pyarn -Phadoop-2.7 -Dhadoop.version=2.7.3 -Phive -Phive-thriftserver -Dscala-2.11 -DskipTests clean install
```

Then, as mentioned in Spark official documents `Building Spark` says:

```bash
$ cd core
$ ../build/mvn scala:cc
```

Also setup the `SPARK_SCALA_VERSION`.
