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



