---
layout: post
title: "Tips on Spark & Hadoop 2.2.0 installation"
description: ""
categories: 
tags: []
---
{% include JB/setup %}


Under most configurations the a Spark cluster's input comes from HDFS (a standalone Spark could use local file system). So the first step to build a Spark cluster is setting up a Hadoop cluster. 

As I haven't read through the requirements for Spark carefully, I just choose to install the Hadoop 2.2.0, which is mentioned in the README.md file. 

Unfortunately, Hadoop 2.2.0 has some bugs. First of all, if you download a pre-build binary package, and run it on 64bit platform, you will get a ugly WARNING:

	INFO util.NativeCodeLoader - Unable to load native-hadoop library for your platform... using builtin-java classes where applicable