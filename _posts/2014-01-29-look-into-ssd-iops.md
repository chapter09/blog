---
layout: post
title: "Look into SSD IOPS"
description: ""
categories: 
tags: []
---

Using IOPS measurement script from [iops.py](http://benjamin-schweizer.de/files/iops/iops-2010-08-12)

* Kingston:

		=== START OF INFORMATION SECTION ===
		Device Model:     KINGSTON SVP200S37A120G
		Serial Number:    50026B7228017241
		Firmware Version: 502ABBF0
		User Capacity:    120,034,123,776 bytes
		Device is:        Not in smartctl database [for details use: -P showall]
		ATA Version is:   8
		ATA Standard is:  Not recognized. Minor revision code: 0x0110
		Local Time is:    Wed Jan 29 14:53:30 2014 HKT
		SMART support is: Available - device has SMART capability.
		SMART support is: Enabled
	
	$ iops.py -n 1 -t 2 /dev/sdb1

	
		/dev/sdb4,  34.60 GB, 1 threads:
		 512   B blocks: 5641.4 IO/s,   2.8 MiB/s ( 23.1 Mbit/s)
		   1 KiB blocks: 5309.6 IO/s,   5.2 MiB/s ( 43.5 Mbit/s)
		   2 KiB blocks: 4621.7 IO/s,   9.0 MiB/s ( 75.7 Mbit/s)
		   4 KiB blocks: 3733.9 IO/s,  14.6 MiB/s (122.4 Mbit/s)
		   8 KiB blocks: 3631.9 IO/s,  28.4 MiB/s (238.0 Mbit/s)
		  16 KiB blocks: 3029.5 IO/s,  47.3 MiB/s (397.1 Mbit/s)
		  32 KiB blocks: 2178.8 IO/s,  68.1 MiB/s (571.1 Mbit/s)
		  64 KiB blocks: 1478.4 IO/s,  92.4 MiB/s (775.1 Mbit/s)
		 128 KiB blocks:  927.4 IO/s, 115.9 MiB/s (972.5 Mbit/s)
		 256 KiB blocks:  667.4 IO/s, 166.9 MiB/s (  1.4 Gbit/s)
		 512 KiB blocks:  402.1 IO/s, 201.0 MiB/s (  1.7 Gbit/s)
		   1 MiB blocks:  226.4 IO/s, 226.4 MiB/s (  1.9 Gbit/s)
		   2 MiB blocks:  124.2 IO/s, 248.4 MiB/s (  2.1 Gbit/s)
		   4 MiB blocks:   64.9 IO/s, 259.4 MiB/s (  2.2 Gbit/s)
		   8 MiB blocks:   33.6 IO/s, 268.9 MiB/s (  2.3 Gbit/s)
		  16 MiB blocks:   16.9 IO/s, 271.0 MiB/s (  2.3 Gbit/s)
		  32 MiB blocks:    8.4 IO/s, 269.3 MiB/s (  2.3 Gbit/s)
		  64 MiB blocks:    3.5 IO/s, 225.2 MiB/s (  1.9 Gbit/s)
		 128 MiB blocks:    1.9 IO/s, 245.9 MiB/s (  2.1 Gbit/s)
		 256 MiB blocks:    0.9 IO/s, 227.2 MiB/s (  1.9 Gbit/s)
	 
* Intel:
	
		wanghao@sing043:~$ sudo smartctl -a /dev/sdb
		smartctl 5.40 2010-07-12 r3124 [x86_64-unknown-linux-gnu] (local build)
		Copyright (C) 2002-10 by Bruce Allen, http://smartmontools.sourceforge.net
		
		=== START OF INFORMATION SECTION ===
		Model Family:     Intel X25-M SSD
		Device Model:     INTEL SSDSA2M080G2GC
		Serial Number:    CVPO005402YE080BGN
		Firmware Version: 2CV102HA
		User Capacity:    80,026,361,856 bytes
		Device is:        In smartctl database [for details use: -P show]
		ATA Version is:   7
		ATA Standard is:  ATA/ATAPI-7 T13 1532D revision 1
		Local Time is:    Wed Jan 29 14:50:29 2014 HKT
		SMART support is: Available - device has SMART capability.
		SMART support is: Enabled 
		
	$ iops.py -n 1 -t 2 /dev/sdb1
		 
		 /dev/sdb1,  80.02 GB, 1 threads:
		 512   B blocks: 13094.4 IO/s,   6.4 MiB/s ( 53.6 Mbit/s)
		   1 KiB blocks: 10974.7 IO/s,  10.7 MiB/s ( 89.9 Mbit/s)
		   2 KiB blocks: 8270.3 IO/s,  16.2 MiB/s (135.5 Mbit/s)
		   4 KiB blocks: 5651.7 IO/s,  22.1 MiB/s (185.2 Mbit/s)
		   8 KiB blocks: 5434.9 IO/s,  42.5 MiB/s (356.2 Mbit/s)
		  16 KiB blocks: 4091.9 IO/s,  63.9 MiB/s (536.3 Mbit/s)
		  32 KiB blocks: 2750.0 IO/s,  85.9 MiB/s (720.9 Mbit/s)
		  64 KiB blocks: 1657.7 IO/s, 103.6 MiB/s (869.1 Mbit/s)
		 128 KiB blocks:  967.2 IO/s, 120.9 MiB/s (  1.0 Gbit/s)
		 256 KiB blocks:  586.4 IO/s, 146.6 MiB/s (  1.2 Gbit/s)
		 512 KiB blocks:  358.6 IO/s, 179.3 MiB/s (  1.5 Gbit/s)
		   1 MiB blocks:  199.3 IO/s, 199.3 MiB/s (  1.7 Gbit/s)
		   2 MiB blocks:  105.3 IO/s, 210.6 MiB/s (  1.8 Gbit/s)
		   4 MiB blocks:   56.4 IO/s, 225.5 MiB/s (  1.9 Gbit/s)
		   8 MiB blocks:   26.5 IO/s, 212.3 MiB/s (  1.8 Gbit/s)
		  16 MiB blocks:   14.4 IO/s, 231.0 MiB/s (  1.9 Gbit/s)
		  32 MiB blocks:    6.3 IO/s, 201.2 MiB/s (  1.7 Gbit/s)
		  64 MiB blocks:    3.6 IO/s, 227.3 MiB/s (  1.9 Gbit/s)
		 128 MiB blocks:    1.7 IO/s, 223.0 MiB/s (  1.9 Gbit/s)
		 256 MiB blocks:    1.0 IO/s, 255.7 MiB/s (  2.1 Gbit/s)