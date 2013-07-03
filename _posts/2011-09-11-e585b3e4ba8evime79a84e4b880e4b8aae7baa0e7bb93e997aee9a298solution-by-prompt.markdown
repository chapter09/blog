---
author: chapter09
comments: true
date: 2011-09-11 15:53:56+00:00
layout: post
slug: '%e5%85%b3%e4%ba%8evim%e7%9a%84%e4%b8%80%e4%b8%aa%e7%ba%a0%e7%bb%93%e9%97%ae%e9%a2%98solution-by-prompt'
title: 关于Vim的一个纠结问题[solution by prompt]
wordpress_id: 188
categories:
- Emotion
tags:
- Linux
- Vim
---

[[回复本文](https://bbs.sjtu.edu.cn/bbspst?board=GNULinux&file=M.1315705202.A)][[原帖](https://bbs.sjtu.edu.cn/bbscon?board=GNULinux&file=M.1315705202.A)]

发信人: [wanghaoeight](https://bbs.sjtu.edu.cn/bbsqry?userid=wanghaoeight)(阿弥喜欢陀佛), 信区: GNULinux

标  题: 关于Vim的一个纠结问题

<!-- more -->

发信站: 饮水思源 (2011年09月11日09:40:02 星期天)

鄙人有轻微强迫症，每次写代码不喜欢写着写着光标到了窗口最底下一行，这个时候就回 车回车留出些空白。 有没有什么设置，然后留出的空白能自动随着代码的换行也换行，从而不需要不断补空白 。。。

-- 阿  陀佛

[http://chapter09.sinaapp.com/](http://chapter09.sinaapp.com/)

发信人: prompt ([光明作使@嘉7如梦 ~]$ ), 信区: GNULinux

标  题: Re: 关于Vim的一个纠结问题

发信站: 饮水思源 (2011年09月11日12:31:12 星期天), 转信

在vimrc添加这样的内容  效果是在insert mode如果光标所在行不是屏幕中间行

输入回车之后，会自动调整当前行为屏幕中间行  如果想要其他位置可以修改下zz那个命令

哎。。弱爆了。。。搞了这么久。。  ==============================================================

function! MyEnter()

execute "normal zz"   return "<CR>"

endfunction

inoremap <CR> <C-R>=call(function("MyEnter"),[])<CR> ==============================================================

【 在 prompt ([光明作使@嘉7如梦 ~]$ ) 的大作中提到: 】

: 自动可以写vimscript解决，我在研究中。。 

: 【 在 KiddingKitte (KiddingKitten) 的大作中提到: 】 

: : 人家要的是自动的吧。 



-- We reject: kings, presidents and voting We believe in: rough consensus and running code 

我有两个好朋友，一个是[RTFM](https://bbs.sjtu.edu.cn/bbsqry?userid=RTFM)，另一个是[STFW](https://bbs.sjtu.edu.cn/bbsqry?userid=STFW) "

我还要不断地学习" 禁可乐，禁烧烤 

Rawbl Yvsr naq Unccl Unpxvat (by rot13)


