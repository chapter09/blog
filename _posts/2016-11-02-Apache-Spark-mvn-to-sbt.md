---
layout: post
title: "Apache Spark mvn to sbt"
description: ""
categories: 
tags: []
---

You cannot find a `build.sbt` file in Apache Spark's source directory, while you can absolutely build Spark with `sbt`. Why? Spark uses a `sbt` plugin `sbt-pom-reader` to read in `pom.xml` used by `mvn`. 

It is still necessary to know how to transfer `build.sbt` to `pom.xml` or reverse. To add extra dependencies with incremental compilation by `sbt`, you have to add `XML` into `pom.xml` rather than create a `build.sbt`.

For example, I have to add [`scala-redis`](https://github.com/debasishg/scala-redis) to Spark core. `scala-redis` only provides `sbt` style installation:

```scala
libraryDependencies ++= Seq(
    "net.debasishg" %% "redisclient" % "3.2"
)
```




https://mvnrepository.com/artifact/net.debasishg/redisclient_2.11/3.2