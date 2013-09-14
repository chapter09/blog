---
layout: post
title: "uid vs euid"
description: ""
category: Linux
tags: [Linux]
---
{% include JB/setup %}


当一个用户登陆系统时，系统会将UID和EUID都赋值为/etc/passwd文件中的UID，一般情况下2个ID是相同的，但是某些情况下会出现2个ID不同的情况。

* UID

	Read UID. It is of the user/process that created THIS process. It can be changed only if the running process has EUID=0.
	
* EUID

	Effective UID. It is used to evaluate privileges of the process to perform a particular action. EUID can be change either to RUID, or SUID if EUID!=0. If EUID=0, it can be changed to anything.
* SUID 

	If the binary image file, that was launched has a Set-UID bit on, SUID will be the UID of the owner of the file. Otherwise, SUID will be the RUID.
	
		$ ls -l shadow
		-rw-r----- 1 root shadow 978 2009-02-22 21:25 shadow
		
		 ls -l passwd
		-rwsr-xr-x 1 root root 32988 2008-06-10 02:10 passwd
		
setuid，setgid，sticky的八进制位分别是4, 2, 1，助记法表示为u+s，g+s，o+t，(删除标记位是u-s，g-s，o-t)。

可以用 ls -l 来查看权限. 如果有这些标志, 则会在原来的执行标志位置上显示. 如 

	rwsrw-r--       表示有setuid标志 
	rwxrwsrw-       表示有setgid标志 
	rwxrw-rwt       表示有sticky标志 