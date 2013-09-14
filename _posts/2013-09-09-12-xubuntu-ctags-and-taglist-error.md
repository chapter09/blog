---
layout: post
title: "xubuntu: ctags and taglist error"
description: ""
category: 
- Linux
tags: []
---
{% include JB/setup %}

taglist插件只支持``exuberant ctags tool``，不支持``GNU ctags``或``UNIX ctags``，Mac或Ubuntu下自带的并不是exuberant ctags，所以执行TlistToggle vim报错，解决方案如下：

* 下载安装 exuberant ctags tool
* ``.vimrc`` 添加 ``let Tlist_Ctags_Cmd = '/usr/bin/ctags' ``
