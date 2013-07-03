---
author: chapter09
comments: true
date: 2012-02-16 08:15:58+00:00
layout: post
slug: openstack%e5%86%85%e4%bf%ae%e6%94%b9vm%e9%85%8d%e7%bd%ae
title: 修改OpenStack内VM配置
wordpress_id: 553
categories:
- Tech
tags:
- CPU
- libvirt
- Nehalem
- openstack
- VM
---

openstack中nova建立VM有多个API，具体调用哪一个需要看代码。
诸多API，如euca2ool, libvirt带来的效果就是耦合性很差。<!-- more -->
由于需要作CPU的实验，因此需要修改Openstack内部VM的配置，同时不影响该VM的其他配置。
我需要对instance-00000001添加如下配置：

[code lang="xml"]

        Nehalem
<!--
       <feature policy='require' name='x2apic' />
                <cpuid function='0x00000001' ecx='0x00200000' />
        </feature>
     <feature policy='require' name='sse4.2' />
                <cpuid function='0x00000001' ecx='0x00100000' />
        </feature>

    <feature policy='require' name='sse4.1' />
                <cpuid function='0x00000001' ecx='0x00800000' />
        </feature>
-->


[/code]


我们可以通过正常的方式配置好VM并启动，由于生成的VM是通用型号的虚拟CPU，可能不具备某些指令集，因此我们需要自行添加配置。
由于不太懂libvirt和euca2ool的程序结构，于是只好硬来。
最后发现可以通过**virsh define **来实现。
但是xml不是**/var/lib/nova/instances/**下面的，如果define这个下面的xml文件，会报错
应该是**/etc/libvirt/qemu**
可以先改完xml文件后，再virsh define
然后重启VM，即可实现VM配置修改
这样修改的粒度很小，也比较方便。只是稍有hack Openstack的嫌疑。。。
