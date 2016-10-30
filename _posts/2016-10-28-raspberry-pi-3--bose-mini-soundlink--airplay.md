---
layout: post
title: "Raspberry Pi 3 + Bose Mini SoundLink + AirPlay"
description: ""
categories: 
tags: []
---

I have a Bose Mini SoundLink, which only supports Bluetooth. It is not convenient when I want to switch between my devices (_e.g._, from my MacBook to my iPhone, or from my iPhone to my iPad) --- I always have to release the Bluetooth connection first (press the button on the SoundLink), then pair the next device, which may fail sometimes.

Fortunately, I have a Raspberry Pi 3, which is equipped with both Wi-Fi and Bluetooth. Then I am wondering I could launch a AirPlay server on Pi 3, while Pi 3 connects to the SoundLink persistently. All my devices can easily switch with AirPlay, rather then Bluetooth.

```
devices <---AirPlay---> Pi 3 <---Bluetooth---> SoundLink
```

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [1. Pi 3 connecting to a Bluetooth Speaker](#1-pi-3-connecting-to-a-bluetooth-speaker)
- [2. Install AirPlay on Pi 3](#2-install-airplay-on-pi-3)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

### 1. Pi 3 connecting to a Bluetooth Speaker

First of all, upgrade the OS of Pi 3 to the latest:

```bash
sudo apt-get update
sudo apt-get dist-upgrade
sudo rpi-update
```

Then, install Bluetooth and audio drivers:

```bash
sudo apt-get install pi-bluetooth blueman
sudo apt-get install pulseaudio pavucontrol pulseaudio-module-bluetooth
```

Launch `pulseaudio` by run:

```bash
pulseaudio -D
```

and then connect to the SoundLink via Bluetooth via `bluetoothctl`:

```bash
❯ bluetoothctl
[bluetooth]# agent on
[bluetooth]# scan on
[NEW] Device xx:xx:xx:xx:xx:xx Bose Mini SoundLink
[bluetooth]# trust xx:xx:xx:xx:xx:xx
[bluetooth]# connect xx:xx:xx:xx:xx:xx
Attempting to connect to xx:xx:xx:xx:xx:xx
[CHG] Device xx:xx:xx:xx:xx:xx Connected: yes
Connection successful
```

### 2. Install AirPlay on Pi 3



