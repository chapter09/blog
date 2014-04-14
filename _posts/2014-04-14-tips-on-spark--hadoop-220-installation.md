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

Unfortunately, Hadoop 2.2.0 has some bugs. First of all, if you download a pre-build binary package, and run it on 64bit platform, you will get a ugly INFO when you run every Hadoop command:

	INFO util.NativeCodeLoader - Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
	
Why? Because the pre-build package is prepared for the 32bit system. So we need to build Hadoop 2.2.0 from its source code.

To build the source code, the Hadoop-10110.patch is needed.

In addition, some libraries are required to be installed. Such as:
	
* libssl-devel
* protoc-2.5.0 (build from source code, and `sudo ldconfig` should be executed in the end.)

More details could refer to [http://blog.csdn.net/linshao_andylin/article/details/12307747](http://blog.csdn.net/linshao_andylin/article/details/12307747) and Google...