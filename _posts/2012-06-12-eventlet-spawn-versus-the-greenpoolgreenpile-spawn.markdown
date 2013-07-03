---
author: chapter09
comments: true
date: 2012-06-12 12:24:03+00:00
layout: post
slug: eventlet-spawn-versus-the-greenpoolgreenpile-spawn
title: eventlet.spawn versus the greenpool/greenpile.spawn
wordpress_id: 754
categories:
- Tech
tags:
- Python
---

What's the difference between the eventlet.spawn and the greenpool/greenpile.spawn?
In my opinion, eventlet.spawn starts GreenThread in the whole or huge environment, while the later one starts GreenThread in a ruled-size pool or pile.
So the pool or pile establishes a domain for the GreenThreads? Under this domain, all the GreenThreads could be controlled by one "hub"?
I need ask my colleague tomorrow...

