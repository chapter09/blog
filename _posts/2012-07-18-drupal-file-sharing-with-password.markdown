---
author: chapter09
comments: true
date: 2012-07-18 18:03:53+00:00
layout: post
slug: drupal-file-sharing-with-password
title: Drupal file sharing with password
wordpress_id: 853
categories:
- Tech
tags:
- Drupal
---

第一次用drupal建站，需要实现一个密码保护文档下载的module，找到了Protected Node:[http://drupal.org/project/protected_node](http://drupal.org/project/protected_node)

但是初次安装就出现了问题，当对该module进行configure的时候，页面就进入403了，我在想是不是d7下该module是个dev版，不会这么坑爹吧。后来在gsj同学的点拨下查看httpd的日志文件，发现是一个php错误，找不到函数定义，于是搜寻该函数，找到了D7下protected node的需要添加一个function在/var/www/html/sites/all/modules/protected_node/protected_node.settings.inc


[http://api.drupal.org/api/drupal/modules!user!user.module/function/_user_password_dynamic_validation/6](http://api.drupal.org/api/drupal/modules!user!user.module/function/_user_password_dynamic_validation/6)




在添加完后，protected node正常了。但是配好密码什么的，发现退出Drupal Admin后，看不多密码框，因为初次使用对Drupal的permission 和role的设置不熟悉，折腾了很久，包括半路去使用了node access password。。。最后发现是需要添加对匿名用户的Protected Node的可查看权限。




坑爹的Drupal 文档。。。毫无Debug机会啊。。。作为程序员不给debug机会怎么有活路啊。。。




误打误撞，希望对后来人有所帮助。





本文借鉴[http://xinm.com/node/303](http://xinm.com/node/303)，感谢[http://xinm.com/](http://xinm.com/)博主的高效回信和帮助

[![](http://haow.ca/wp-content/uploads/2012/07/Capture2.jpg)](http://haow.ca/wp-content/uploads/2012/07/Capture2.jpg)
