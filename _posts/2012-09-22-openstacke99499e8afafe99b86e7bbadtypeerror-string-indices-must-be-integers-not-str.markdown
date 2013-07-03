---
author: chapter09
comments: true
date: 2012-09-22 14:53:19+00:00
layout: post
slug: openstack%e9%94%99%e8%af%af%e9%9b%86%e7%bb%adtypeerror-string-indices-must-be-integers-not-str
title: '[OpenStack错误集续]TypeError: string indices must be integers, not str '
wordpress_id: 886
categories:
- Tech
tags:
- ''''
- OpenSource
- openstack
---

在Essex中依旧遇到这个问题了：

[code lang="python"]
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


[https://answers.launchpad.net/nova/+question/202734](https://answers.launchpad.net/nova/+question/202734)中：








[Xiaolin Zhang (zhangxiaolins)](https://launchpad.net/~zhangxiaolins) said on 2012-08-17:


#3








need to update api-paste.ini, and ensure the following lines are present:


admin_tenant_name = service
admin_user = nova
admin_password = nova






然后问题的确在此。


