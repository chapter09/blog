---
author: chapter09
comments: true
date: 2012-02-21 06:38:01+00:00
layout: post
slug: speccpu-%e6%8a%a5%e9%94%99%ef%bc%9aexecvp
title: SpecCPU 报错：execvp
wordpress_id: 569
categories:
- Tech
tags:
- Error
- openstack
- SpecCPU
---

在一台VM上跑SpecCPU，得到以下错误。
根据execvp想到gcc的问题，然后查询gcc和libgc是否安装完好
发现都没有问题。
最后发现/lib/下少了很多库文件，多亏有一台已经正常运行的VM，两相对比才发现缺少的东西

[css autolinks="false" collapse="false" firstline="1" gutter="false" highlight="1-3,6,9" htmlscript="false" light="false" padlinenumbers="false" smarttabs="true" tabsize="4" toolbar="true" title="CPU2006.log" wraplines="true" lang="bash"]
****************************************
Contents of checkspam.2500.5.25.11.150.1.1.1.1.err
****************************************
execvp: No such file or directory
failed to execute ../run_base_ref_cpu2006.1.2.ic12.1rc1.linux64.sse42.rate.17aug2011.0000/
        perlbench_base.cpu2006.1.2.ic12.1rc1.linux64.sse42.rate.17aug2011
****************************************

[/css]
以下是lib下的文件列表：<!-- more -->
[css autolinks="false" collapse="false" firstline="1" gutter="false" highlight="1-3,6,9" htmlscript="false" light="false" padlinenumbers="false" smarttabs="true" tabsize="4" toolbar="true" title="CPU2006.log" wraplines="true" lang="bash"]

alsa            libanl.so.1              libc.so.6                     
libm.so.6             libnss_files.so.2       libpthread.so.0      libutil-2.12.so
cpp             libBrokenLocale-2.12.so  libdl-2.12.so                 
libnsl-2.12.so         libnss_hesiod-2.12.so   libresolv-2.12.so    libutil.so.1
crda            libBrokenLocale.so.1     libdl.so.2                    
libnsl.so.1            libnss_hesiod.so.2      libresolv.so.2       lsb
firmware        libc-2.12.so             libfreebl3.chk                
libnss_compat-2.12.so  libnss_nis-2.12.so      librt-2.12.so        modules
kbd             libcidn-2.12.so          libfreebl3.so                
 libnss_compat.so.2     libnss_nisplus-2.12.so  librt.so.1           security
ld-2.12.so      libcidn.so.1             libgcc_s-4.4.5-20110214.so.1  
libnss_dns-2.12.so     libnss_nisplus.so.2     libSegFault.so       terminfo
ld-linux.so.2   libcrypt-2.12.so         libgcc_s.so.1                 
libnss_dns.so.2        libnss_nis.so.2         libthread_db-1.0.so  udev
libanl-2.12.so  libcrypt.so.1            libm-2.12.so                  
libnss_files-2.12.so   libpthread-2.12.so      libthread_db.so.1

[/css]


