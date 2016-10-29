---
layout: post
title: "How does Linux change default Java"
description: ""
categories: 
tags: []
---

How to change the default JAVA for Linux. Actually Linux offers a command to settle down default values including JAVA.

	sudo update-alternatives --remove-all java
	sudo update-alternatives --remove-all javac
	sudo update-alternatives --remove-all javaws
	
Now when trying to install new alternatives I get the following:

	sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk/bin/java 1
