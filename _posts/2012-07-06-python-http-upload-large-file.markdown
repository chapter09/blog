---
author: chapter09
comments: true
date: 2012-07-06 02:46:57+00:00
layout: post
slug: python-http-upload-large-file
title: Python HTTP upload large file
wordpress_id: 824
categories:
- Tech
tags:
- HTTP
- Python
---

When I go through the related questions on StackOverFlow, I find it's hard find some sample code to upload large files via HTTP in Python. General methods to upload will read the whole file into your memory, once the file is up to 2 GB or more, your client will be down.

For a lot of libraries, urllib, urllib2, it seems that they just support small file upload, as they can not handle operations in small granularity like httplib.

httplib offers methods to invoke to set up the http connection, make the http header and the most important thing is that httplib supports upload chunked file with its send() method. After reading the example below you will get a clear understand.

**client:**

[code lang="python"]
import httplib
import sys
import mimetypes
import os
CHUNKSIZE = 65563	
url="http://127.0.0.1:8090/"
conn = httplib.HTTPConnection("127.0.0.1", 8090)
conn.putrequest("PUT", "/?file=MONACO.TTF")
conn.putheader("Content-Type", "application/zip")
conn.putheader("Transfer-Encoding", "chunked")
conn.endheaders()

fp = 'C:\Users\haow\Downloads\ubuntu-11.10-server-amd64.zip'
# fp = 'C:\Users\haow\Downloads\MONACO.TTF'
f = os.path.basename(fp)
print f
print mimetypes.guess_type(f)

with open(f, 'rb') as fp:
	chunk = fp.read(CHUNKSIZE)
	while chunk:
		conn.send('%xrn%srn' % (len(chunk), chunk))
		chunk = fp.read(CHUNKSIZE)
	conn.send('0rnrn')
response = conn.getresponse()
print response.read()
[/code]

**server:**
[code lang="python"]
def hello_world(environ, start_response):
    CHUNKSIZE = 65563
    request = Request(environ)
    fname = request.args["file"]
    f = open(fname, "wb")
    chunk = environ["wsgi.input"].read(CHUNKSIZE)
    while chunk:
        f.write(chunk)
        chunk = environ["wsgi.input"].read(CHUNKSIZE)
    f.close()
    response = Response("success", mimetype='plain/text')
    return response(environ, start_response)

if __name__ == "__main__":
    wsgi.server(eventlet.listen(('127.0.0.1', 8090)), hello_world)
[/code] 
