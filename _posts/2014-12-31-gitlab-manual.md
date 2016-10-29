---
layout: post
title: "GitLab Manual"
description: ""
categories: 
tags: []
---

###1. Deploy SSH Key in your profile

1. Please click your profile settings button as follow:

![image](http://api.drp.io/files/54a3fe6e9215f.png)

2. Generate SSH key-pair by command `ssh-keygen`, and copy the public key to your profile. 

3. In your profile, you could find the `SSH Keys` tab, press the `Add SSH Key` button then GitLab could identify you (which means you could push/pull code).

![image](http://api.drp.io/files/54a3feba7605e.png)

_P.S.: The Key Deployment in the `project settings` only offer a way to read-only access to the repository. To fully access to any repository, public key should be deployed in everyone's `personal settings`._

###2. Git Address

Because our git server is deployed behind the gateway, we need to change the git address to external address:
    
The GitLab will display the git address as:

    git remote add origin git@192.168.1.11:Hao/test.git

Then, please change the git address to the following format:
    
    git remote add origin ssh://git@sing.cse.ust.hk:30011/Hao/test.git

Thus, Pascal's Mind project will be
    
    git remote add origin ssh://git@sing.cse.ust.hk:30011/poupartp/mind.git

