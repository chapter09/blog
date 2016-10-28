---
author: chapter09
comments: true
date: 2011-09-11 15:53:56+00:00
layout: post
title: 关于Vim的一个纠结问题[solution by prompt]
categories:
- Emotion
tags:
- Linux
- Vim
---

发信人: [wanghaoeight](https://bbs.sjtu.edu.cn/bbsqry?userid=wanghaoeight)(阿弥喜欢陀佛), 信区: GNULinux

标  题: 关于Vim的一个纠结问题

发信站: 饮水思源 (2011年09月11日09:40:02 星期天)

鄙人有轻微强迫症，每次写代码不喜欢写着写着光标到了窗口最底下一行，这个时候就回 车回车留出些空白。 有没有什么设置，然后留出的空白能自动随着代码的换行也换行，从而不需要不断补空白 。。。

-- 阿  陀佛

[http://chapter09.sinaapp.com/](http://chapter09.sinaapp.com/)

发信人: prompt ([光明作使@嘉7如梦 ~]$ ), 信区: GNULinux

标  题: Re: 关于Vim的一个纠结问题

发信站: 饮水思源 (2011年09月11日12:31:12 星期天), 转信

在vimrc添加这样的内容  效果是在insert mode如果光标所在行不是屏幕中间行

输入回车之后，会自动调整当前行为屏幕中间行  如果想要其他位置可以修改下zz那个命令

哎。。弱爆了。。。搞了这么久。。  

	
	function! MyEnter()
	execute "normal zz"   return "<CR>"
	endfunction
	inoremap <CR> <C-R>=call(function("MyEnter"),[])<CR> 
	


