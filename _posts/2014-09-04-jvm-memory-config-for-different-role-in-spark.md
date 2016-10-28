---
layout: post
title: "JVM Memory Config For Different Role in Spark"
description: ""
categories: 
tags: []
---
{% include JB/setup %}

<span class="badge">Spark 1.0.1</span>

When I try to `broadcast` a 1 GigaBytes scala List, I meet the common OOM exception "java heap size". Then I realize it is necessary to configure the size of JVM heap size to satify the large variable.

In the `spark-env.sh`, a