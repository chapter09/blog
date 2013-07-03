---
author: chapter09
comments: true
date: 2012-11-05 02:03:42+00:00
layout: post
slug: openstack%e9%94%99%e8%af%af%e9%9b%86%e7%bb%adremove-keystone-error
title: '[OpenStack错误集续]remove Keystone error'
wordpress_id: 911
categories:
- Tech
tags:
- openstack
---

Error:
[code]
 File "/usr/local/bin/keystone-manage", line 5, in <module>
    pkg_resources.run_script('keystone==2012.1', 'keystone-manage')
  File "/usr/lib/python2.7/dist-packages/pkg_resources.py", line 499, in run_script
    self.require(requires)[0].run_script(script_name, ns)
  File "/usr/lib/python2.7/dist-packages/pkg_resources.py", line 1235, in run_script
    execfile(script_filename, namespace, namespace)
  File "/usr/local/lib/python2.7/dist-packages/keystone-2012.1-py2.7.egg/EGG-INFO/scripts/keystone-manage", line 28, in <module>
    cli.main(argv=sys.argv, config_files=config_files)
  File "/usr/local/lib/python2.7/dist-packages/keystone-2012.1-py2.7.egg/keystone/cli.py", line 147, in main
    return run(cmd, (args[:1] + args[2:]))
  File "/usr/local/lib/python2.7/dist-packages/keystone-2012.1-py2.7.egg/keystone/cli.py", line 133, in run
    return CMDS[cmd](argv=args).run()
  File "/usr/local/lib/python2.7/dist-packages/keystone-2012.1-py2.7.egg/keystone/cli.py", line 35, in run
    return self.main()
  File "/usr/local/lib/python2.7/dist-packages/keystone-2012.1-py2.7.egg/keystone/cli.py", line 54, in main
    driver = utils.import_object(getattr(CONF, k).driver)
  File "/usr/local/lib/python2.7/dist-packages/keystone-2012.1-py2.7.egg/keystone/common/utils.py", line 60, in import_object
    cls = import_class(import_str)
  File "/usr/local/lib/python2.7/dist-packages/keystone-2012.1-py2.7.egg/keystone/common/utils.py", line 47, in import_class
    __import__(mod_str)
ImportError: No module named rules
dpkg: error processing keystone (--configure):
 subprocess installed post-installation script returned error exit status 1
E: Sub-process /usr/bin/dpkg returned an error code (1)
[/code]
我的做法是：
直接
 rm -fr /usr/local/lib/python2.7/dist-packages/keystone-2012.1-py2.7.egg/
然后重新apt-get install keystone即可




