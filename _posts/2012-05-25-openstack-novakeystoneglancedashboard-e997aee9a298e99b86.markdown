---
author: chapter09
comments: true
date: 2012-05-25 11:29:25+00:00
layout: post
slug: openstack-novakeystoneglancedashboard-%e9%97%ae%e9%a2%98%e9%9b%86
title: Nova+Keystone+Glance+Dashboard 问题集
wordpress_id: 715
categories:
- Tech
tags:
- openstack
---

[OpenStack各个组件版本]

nova-api_2011.3+git20111117-0ubuntu1_all.deb

nova-common_2011.3+git20111117-0ubuntu1_all.deb

nova-network_2011.3+git20111117-0ubuntu1_all.deb

nova-objectstore_2011.3+git20111117-0ubuntu1_all.deb

nova-scheduler_2011.3+git20111117-0ubuntu1_all.deb

nova-vncproxy_2011.3+git20111117-0ubuntu1_all.deb

nova-volume_2011.3+git20111117-0ubuntu1_all.deb

python-nova-adminclient_0.1.8-0ubuntu1_amd64.deb

keystone_1.0~d4~20110909.1108-0ubuntu3.1_all.deb

openstackx_0.2-0ubuntu3_all.deb

python-django-openstack_0.1~20110825-0ubuntu2_all.deb

python-openstack-compute_2.0a1-0ubuntu2_amd64.deb

**python_novaclient-2.6.4.egg-info**

** ** python-keystone_1.0~d4~20110909.1108-0ubuntu3.1_all.deb

openstack-dashboard_0.1~20110825-0ubuntu2_all.deb

1. db sync 的问题，解决：权限设置

2. vlan的配置，解决：理解vlan，同时通过log里的命令理清楚nova在网络配置的设置

3. TypeError: string indices must be integers, not str

[code]

header: Date: Fri, 25 May 2012 11:37:36 GMT
Traceback (most recent call last):
  File "/usr/bin/nova", line 9, in
    load_entry_point('python-novaclient==2.6.4', 'console_scripts', 'nova')()
  File "/usr/lib/python2.7/dist-packages/novaclient/shell.py", line 219, in main
    OpenStackComputeShell().main(sys.argv[1:])
  File "/usr/lib/python2.7/dist-packages/novaclient/shell.py", line 182, in main
    args.func(self.cs, args)
  File "/usr/lib/python2.7/dist-packages/novaclient/v1_1/shell.py", line 238, in do_image_list
    utils.print_list(cs.images.list(), ['ID', 'Name', 'Status'])
  File "/usr/lib/python2.7/dist-packages/novaclient/v1_1/images.py", line 45, in list
    return self._list("/images/detail", "images")
  File "/usr/lib/python2.7/dist-packages/novaclient/base.py", line 71, in _list
    data = body[response_key]
TypeError: string indices must be integers, not str
[/code]

keystone的endpointTemplates 设为novahost:8774

而非8773 8774如果nova-api没有监听则检查nova.conf,

笔者的错误就是将nova.conf中设置了osapi_listen_port=8774去掉这条语句即可。

4.nova-network.log Permission denied: '/var/lib/nova/networks/nova-br100.conf' chown -R nova:nova /var/lib/nova/ 权限问题。。。

5. nova-network reporting NoMoreNetworks  drop数据库，重新nova-manage network create private .....

6.libvirtError: internal error no supported architecture for os type 'hvm'

7.libvirtError: internal error cannot create rule since ebtables tool is missing  暂时改成libvirt_type=qemu  
