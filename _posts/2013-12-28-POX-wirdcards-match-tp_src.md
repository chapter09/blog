---
layout: post
title: "OpenFlow Table Miss"
description: ""
categories: 
tags: []
---
{% include JB/setup %}

controller需要通过tp_src 来match switch上的一条rule并做DELETE操作，但是单纯以一下code:

	msg_updt.match = of.ofp_match()
	msg_updt.match.tp_src = self.flow_list[flow_rm]

是会有warning的：

	WARNING:libopenflow_01:Fields ignored due to unspecified prerequisites:  tp_src 
	xxx
	core:Down
	
综合POX-dev邮件列表的邮件和文档：

> The problem is that you haven't matched on ethertype = ip - if you don't do this explicitly, then the switch rejects the flow. 

-- <https://www.mail-archive.com/pox-dev@lists.noxrepo.org/msg00588.html>

> I tried to install a table entry but got a different one.  Why?
> 
This question also presents itself as "What does the 'Fields ignored due to unspecified prerequisites' warning mean?"

Basically this means that you specified some higher-layer field without specifying the corresponding lower-layer fields also.  For example, you may have tried to create a match in which you specified only tp_dst=80, intending to capture HTTP traffic.  You can't do this.  To match TCP port 80, you must also specify that you intend to match TCP (nw_proto=6).  And in order to match on TCP, you must also match on IP (dl_type=0x800).
For more information, see the text on "normal form" flow descriptions in the ovs-ofctl man page, or the new clarifying text added to section 3.4 in the OpenFlow 1.0.1 specification.

应该提供对packet 更底层layer的match field:
  
  msg_updt.match = of.ofp_match()
  msg_updt.match.dl_type = 0x0800
  msg_updt.match.nw_proto = 6
  msg_updt.match.tp_src = self.flow_list[flow_rm]
 
 这样就OK了。
 
剩下的问题就是如何从OFPFC_DELETE和OFPFC_DELETE_STRICTLY找到精确删除rule的方式。目前采用OFPFC_DELETE则是删太多，而后者则是可能不删。。。