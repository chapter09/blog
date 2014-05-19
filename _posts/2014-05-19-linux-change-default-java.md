---
layout: post
title: "Linux change default Java"
description: ""
categories: 
tags: []
---
{% include JB/setup %}


Corresponding x64 tag.gz archive for my architecture has been downloaded and extracted to /usr/lib/jvm as usual. All previous versions of Java were installed before the same way. But before setting new alternatives for java, javac and javaws I removed all existing alternatives using the following commands:

	sudo update-alternatives --remove-all java
	sudo update-alternatives --remove-all javac
	sudo update-alternatives --remove-all javaws
Now when trying to install new alternatives I get the following:

	sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk/bin/java 1

update-alternatives: error: alternative path /usr/bin/java doesn't exist.
Of course, /usr/bin/java doesn't exist but /usr/bin does? What's wrong with it and how can I fix it?