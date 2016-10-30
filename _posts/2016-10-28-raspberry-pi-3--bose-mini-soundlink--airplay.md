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
- [2. Install AirPlay (shairport-sync) on Pi 3](#2-install-airplay-shairport-sync-on-pi-3)
- [3. Config the sound source/sink on `pulseaudio`](#3-config-the-sound-sourcesink-on-pulseaudio)
- [4. Conclusion](#4-conclusion)

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

Now we have Pi 3 connected to the SoundLink.

### 2. Install AirPlay (shairport-sync) on Pi 3

We deploy an open source AirPlay service ([shairport-sync](https://github.com/mikebrady/shairport-sync)) on Pi 3. 

The while installation is very easy, please refer to its documents on GitHub.

Some threads talking about what is the best implementation of AirPlay on Pi 3 can be found [here] (https://www.raspberrypi.org/forums/viewtopic.php?f=35&t=124122). [shairport-sync](https://github.com/mikebrady/shairport-sync) is the best. I tried [shairport], which has quite a lot of clutters in sound and has been out of maintainence for a long time.

Another error I have met is:
```
Error: ALSA lib pcm.c:2217:(snd_pcm_open_noupdate) Unknown PCM cards.pcm.front
```

To walk around, modify `/usr/share/alsa/alsa.conf` as follows:

```
pcm.front cards.pcm.default
# pcm.front cards.pcm.front
```

Furthermore, [Here](https://gist.github.com/chapter09/a9513640035b754813c0bb4e240f6f66) is my `shairport-sync` configuration file.


### 3. Config the sound source/sink on `pulseaudio`

Set `pulseaudio` sink to the Bluetooth device --- the SoundLink:

```
pacmd list-sinks
pacmd set-default-sink bluez_sink.xx_xx_xx_xx_xx_xx
```

### 4. Conclusion

When all aboves are settled down, you could have:

<img src="https://i.imgsafe.org/5930cdae4b.png" alt="demo" style="width: 50%;"/>










