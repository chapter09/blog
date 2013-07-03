---
author: chapter09
comments: true
date: 2012-08-01 06:37:54+00:00
layout: post
slug: pymox-mocks-urllib2-with-eventlet-green-patched
title: pymox Mocks urllib2 with eventlet.green patched
wordpress_id: 860
categories:
- Tech
tags:
- pymox
- Python
- unit test
---

To do unit test, I need to mock the urllib2.openurl function, but unfortunately got this Error:

[code lang="python"]
File "C:Python27libsite-packagesmox.py", line 1041, in _VerifyMethodCall
    raise UnexpectedMethodCallError(self, expected)
UnexpectedMethodCallError: Unexpected method call.  unexpected:-  expected:+
- urlopen.__call__(<urllib2.Request instance at 0x00000000030F5888>, timeout=3000) -> None
+ urlopen.__call__(<class mox.IgnoreArg at 0x0000000002CB4048>, <class mox.IgnoreArg at 0x0000000002CB4048>) -> <StringIO.StringIO instance at 0x00000000030E5808>
[/code]

And the unit test code is:
[code lang="python"]
def test_do_requset(self):
        self.mox.StubOutWithMock(urllib2, "urlopen")
        temp = StringIO.StringIO("200")
        urllib2.urlopen(mox.IgnoreArg, mox.IgnoreArg).AndReturn(temp)

        self.mox.ReplayAll()
        fd = self.cal._do_request("127.0.0.1", 3000)
        self.assertEqual(fd, "200")
        self.mox.VerifyAll()
[/code]

How to deal with this problem?
My colleague gives me a advice and it works:
[code lang="python"]
self.mox.StubOutWithMock(urllib2, "urlopen")
        self.mox.StubOutClassWithMocks(urllib2, "Request")
        temp = StringIO.StringIO("200")
        request = urllib2.Request("http://127.0.0.1")
        urllib2.urlopen(request, timeout=3000).AndReturn(temp)
        self.mox.ReplayAll()
        fd = self.cal._do_request("127.0.0.1")
        self.assertEqual(fd, "200")
        self.mox.VerifyAll()
[/code] 


