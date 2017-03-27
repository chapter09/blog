---
layout: post
title: "\"No such file or directory\" even though the executable is there"
description: ""
layout: post
comments: true
categories: 
tags: []
---

The problem seems common when running a 32-bit binary on a 64-bit system.

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
The problem may be result from the file type or dependencies of the executable. So we use `file`, `ld` and `ldd` to analyze the executable first:

```shell
❯ ld asm
ld: i386 architecture of input file `asm' is incompatible with i386:x86-64 output
ld: error in asm(.eh_frame); no .eh_frame_hdr table will be created.
```

```shell
❯ file asm
asm: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.24, BuildID[sha1]=0a16781f2f58f35473dec649fad1b1965b407bcc, not stripped
```

```shell
~/ECE353/blitz/bin
❯ ldd asm
        not a dynamic executable
```
After install `libc6-dev-i386`, we execute `ldd` again. We will have output:

```shell
~/ECE353/blitz/bin
❯ ldd asm
        linux-gate.so.1 =>  (0xf77c1000)
        libm.so.6 => /lib32/libm.so.6 (0xf775e000)
        libc.so.6 => /lib32/libc.so.6 (0xf75aa000)
        /lib/ld-linux.so.2 (0x56573000)
```

The `asm` is a 32-bit executable, compared to the native `htop`:

```shell
/usr/bin
❯ file htop
htop: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=165c78eba08c4884f2af3881cf7999ebd0ed587f, stripped
```

P.S.: For stripped or non-stripped executable files, the main difference is the debugging information is removed from stripped executable files [http://unix.stackexchange.com/a/2972](http://unix.stackexchange.com/a/2972).

#### Reference:
* [Getting “Not found” message when running a 32-bit binary on a 64-bit system](http://unix.stackexchange.com/questions/13391/getting-not-found-message-when-running-a-32-bit-binary-on-a-64-bit-system)
* [No such file or directory? But the file exists!](http://askubuntu.com/a/784600)