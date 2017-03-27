---
layout: post
title: "\"No such file or directory\" even though the executable is there"
description: ""
layout: post
comments: true
categories: 
tags: []
---

#### Problem:
Running an executable file, the terminal returns "no such file or directory". It's weird for me, since there is even no errors.

```shell
~/ECE353/blitz/bin
❯ ls
asm  blitz  check  diskUtil  dumpObj  endian  Hello  hexdump  kpl  lddd

~/ECE353/blitz/bin
❯ ./asm
zsh: no such file or directory: ./asm
```
My environment:

```
Linux hostname 4.4.0-66-generic #87-Ubuntu SMP x86_64 x86_64 x86_64 GNU/Linux
```

#### Solution:
After some search, I found installing the following library could solve this problem.

```
sudo apt-get install lib32stdc++6
```
However, I still don't know why this solution works and how to find this solution at this moment.

#### Hints (Why?):

#### Reference:
* [Getting “Not found” message when running a 32-bit binary on a 64-bit system](http://unix.stackexchange.com/questions/13391/getting-not-found-message-when-running-a-32-bit-binary-on-a-64-bit-system)