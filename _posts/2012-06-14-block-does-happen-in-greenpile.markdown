---
author: chapter09
comments: true
date: 2012-06-14 01:46:28+00:00
layout: post
slug: block-does-happen-in-greenpile
title: Block does happen in GreenPile
wordpress_id: 763
categories:
- Tech
tags:
- Python
---

According to the documentation of the eventlet:

GreenPile is an abstraction representing a bunch of I/O-related tasks.

**spawn**(_func_, _*args_, _**kw_)

Runs _func_ in its own green thread, with the result available by iterating over the GreenPile object.

After read the source code of the eventlet, the block happens in the next() method of GreenPile.

In the class GreenThread:

[code lang="python"]
def main(self, function, args, kwargs):
    try:
        result = function(*args, **kwargs)
    except:
	self._exit_event.send_exception(*sys.exc_info())
	self._resolve_links()
	raise
    else:
        self._exit_event.send(result)
        self._resolve_links()
[/code]

In the method spawn() of class GreenPool, _the spawn() method in GreenPile calls this spawn()_:

[code lang="python"]
if self.sem.locked() and current in self.coroutines_running:
# a bit hacky to use the GT without switching to it
    gt = greenthread.GreenThread(current)
    gt.main(function, args, kwargs)
    return gt
[/code]

In the GreenPile.spawn(), it calls the spawn method in GreenPool, besides the GreenPile has methods __iter__() and next() to makes itself to become a iterator. In the next() method, we could find it returns self.waiters.get().wait().

waiters is eventlet.queue.LightQueue object

GreenThreads are put In the LightQueue.

wait() is the block method.

[code lang="python"]
def __iter__(self):
    return self

def next(self):
    """Wait for the next result, suspending the current greenthread until it is available.  Raises StopIteration when there are no more results."""
    if self.counter == 0 and self.used:
        raise StopIteration()
        try:
            return self.waiters.get().wait()
            finally:
	    self.counter -= 1
[/code] 
