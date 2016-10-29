---
layout: post
title: "Multi SSH Key pairs on one client"
description: ""
categories: 
tags: []
---

If there exist multi SSH key pairs and each pair works on a SSH connection, then we need to configure the `config` file under `~/.ssh/`, here is an example:

	Host sing11
	  User            wanghao
	  HostName        sing11
	  IdentityFile    ~/.ssh/id_rsa
	
	Host sing11
	  User            git
	  Hostname        sing11
	  IdentityFile    ~/.ssh/gitlab_id_rsa
	
	  #Hostname        192.168.1.11
	  #IdentityFile    ~/.ssh/id_rsa