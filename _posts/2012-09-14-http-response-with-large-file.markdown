---
author: chapter09
comments: true
date: 2012-09-14 08:06:26+00:00
layout: post
slug: http-response-with-large-file
title: HTTP response with large file
wordpress_id: 876
categories:
- Tech
tags:
- HTTP
- Python
---

Client: send a GET request to the Server for a large file.  (>2GB)

Server: when receive the request, open the target file, and read in and send chunk by chunk, which means that the memory won't be full. Besides, the client should read the data in response chunk by chunk for the same reason.

I tried werkzeug.wrappers.Response, with its stream property, I think that could save memory. However, Response.stream puts all the chunks into the memory, which is helpless in saving the memory.

code is as below:

Server:

[code lang="python"]
from werkzeug.wrappers import Response, Request
from werkzeug.serving import run_simple

def app(environ, start_response):
request = Request(environ)
fb = {}
fb["status"] = "200"
fb["time"] = time.asctime()
f = open("C:\Users\haow\Downloads\Fedora-16-x86_64-DVD.iso", "rb")
chunk = f.read(10240)
response = Response()

while chunk:
response.stream.write(chunk)
chunk = f.read(10240)
print len(chunk)
f.close()
return response(environ, start_response)

if __name__ == '__main__':
run_simple('localhost', 5000, app, use_reloader=True)
[/code]

I need find **some methods to transfer data via HTTP when keeping the connection alive**, so sending chunk by chunk could be achieved as the send() method in httplib
