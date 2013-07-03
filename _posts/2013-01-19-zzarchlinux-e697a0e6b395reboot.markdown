---
author: chapter09
comments: true
date: 2013-01-19 03:00:10+00:00
layout: post
slug: zzarchlinux-%e6%97%a0%e6%b3%95reboot
title: '[zz]ArchLinux 无法reboot'
wordpress_id: 946
categories:
- Tech
tags:
- ArchLinux
---

### Reboot on Dell Latitude E6520 with Arch linux


http://intosimple.blogspot.jp/2012/06/reboot-on-dell-latitude-e6520-with-arch.html





On Dell Latitude E6520 and some other Latitude models, a number of distributions were having issues with reboot. Rebooting by any method, viz. 



	
  * `restart` command

	
  * `shutdown -r now `command

	
  * or restart from GUI


stopped all processes. The run level goes to 6 and everything works fine. Finally the system shows a message that it is rebooting now but the reboot does not happen. Ubuntu had this[bug](https://bugs.launchpad.net/ubuntu/+source/linux/+bug/833705) too but they fixed it and when my friend tried Ubuntu on the same hardware, reboot worked fine for him. I filed a [bug](https://bugs.archlinux.org/task/30136) in Arch and discussed it on #archlinux. The developers said it is an upstream issue and I was able to find an already reported [bug](https://bugzilla.kernel.org/show_bug.cgi?id=42542) upstream. Looking into the bugs and comments, I figured I should try setting the kernel parameter 'reboot' to 'pci'. So, I changed the kernel line in `/boot/grub/menu.lst` to include 'reboot' parameter as follows.

`kernel /boot/vmlinuz26 root=/dev/sda2 resume=/dev/sda1 ro **reboot=pci**`
**
**
After the above change, my system rebooted successfully.
_ _

    
    <code>/* reboot=b[ios] | s[mp] | t[riple] | k[bd] | e[fi] [, [w]arm | [c][/c]old] | p[ci]
       warm   Don't set the cold reboot flag
       cold   Set the cold reboot flag
       bios   Reboot by jumping through the BIOS (only for X86_32)
       smp    Reboot by executing reset on BSP or other CPU (only for X86_32)
       triple Force a triple fault (init)
       kbd    Use the keyboard controller. cold reset (default)
       acpi   Use the RESET_REG in the FADT
       efi    Use efi reset_system runtime service
       pci    Use the so-called "PCI reset register", CF9
       force  Avoid anything that could hang.
     */</code>


_N.B.: 1. Here, `/dev/sda2` is my root partition and `/dev/sda1` is my swap partition._
_ 2. [Read](http://mjg59.livejournal.com/137313.html) more about rebooting a system._





