---
author: wanghao
comments: true
date: 2013-03-13 12:03:18+00:00
layout: post
slug: csdi_2-virtual-memory
title: 'CSDI_2: Virtual Memory'
wordpress_id: 956
categories:
- Tech
---


	
  * [Flashvm: Virtual memory management on ﬂash.](http://ipads.se.sjtu.edu.cn/courses/osdi/2013/papers/Flashvm-atc-10.pdf) M. Saxena and M. M. Swift. In Proc. USENIX Annual Technical Conference, Boston, MA, June 2010.



	
  * **Questions**

	
  * 1. What are the differences between Flash and traditional disks? List two of them.(Hint: You can compare them from IO performance, pattern of R/W and their different architecture.)

_a. While disks exhibit a range of performance, their fundamental characteristic is the speed difference between random and sequential ac- cess. In contrast, flash devices have a different set of performance characteristics, such as fast random reads, high sequential bandwidths, low access and seek costs, and slower writes than reads.
b. The Flash is made up of NAND flash memory packages but the disk contains a machinery structure with DC Spindle Motor, Head Stack Assembly and Disk Stack Assembly etc.![](http://www.datarecoverytools.co.uk/wp-content/uploads/2009/06/hdd-structure.jpg)
c. In SSD a single block may only be re-written a finite number of times, and this limit is around 10,000 and is decreasing with the increase in capacity and density of MLC flash devices. Above this limit, devices may exhibit unacceptably high bit error rates._

2. FlashVM make a lot of improvements and optimizations. Please list the improvements based on the features you mentioned in your first answers, and explain why it works.
_
However, random reads are inexpensive, so write-back should optimize for write locality rather than read locality. FlashVM accomplishes this by leveraging the **page pre-cleaning** and **clustering mechanisms** in Linux to reduce page write over-heads._
a. Writing more pages at a time to achieve sequential write performance on flash. So for flash, the lower access latency and higher bandwidths enable more aggressive pre-cleaning without affecting the latency for handling a page-fault.
b.To avoid random writes, Linux allocates clusters, which are contiguous ranges of page slots.

	
  * 3. Suppose we have a new device called "Tlash" which has its own features like below:








Device


Random read


Seq read


Radom write


Seq write






Disk


5000


500


5000


500






Flash


100


85


2000


200-500






Tlash


200


100


50


50




How will you design your own TlashVM under this condition? At least, you should mention (1)page write-back and prefetch policy (2) page storage layout

	
  * _For Tlash, random write is more inexpensive than random read, so Page Write-Back should optimize for read locality rather than write locality. Besides, like Flash, Tlash also performances well on random read, so it could adopt the same policy as Flash, like stride prefetching or seeking over the free/bad pages when prefetch. For page storage layout, my opinion is that the recent mostly used pages should be put in sequence, as the Seq Read of Tlash is more inexpensive than Random Read._


