---
author: chapter09
comments: true
date: 2012-03-16 06:27:22+00:00
layout: post
slug: abs-learning
title: ABS learning
wordpress_id: 620
categories:
- Tech
tags:
- Bash
- Shell
---

ABS = Advanced Bash-Scripting

一个挂载和是否格式化的脚本
我迈开了我Bash 编程的第一步

[code lang="bash"]
#!/bin/sh
VOLUME_PATH=$1
MOUNT_POINT=$2
FORMAT=$3

if [ -z "$VOLUME_PATH" ] || [ -z "$MOUNT_POINT" ]
then
        echo "Please enter the /dev/vd* path of volume and the mount point"
else
    if [ "$FORMAT" == "y" ]
        then
                echo "format $VOLUME_PATH"
                echo "y" | mkfs.ext3 $VOLUME_PATH
        fi
    echo "mount $VOLUME_PATH $MOUNT_POINT"
    mount $VOLUME_PATH $MOUNT_POINT
fi
[/code] 
