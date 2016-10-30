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

### 1. Pi 3 connecting to a Bluetooth Speaker

First of all, upgrade the OS of Pi 3 to the latest:
```
sudo apt-get update
sudo apt-get dist-upgrade
sudo rpi-update
```

Then, install Bluetooth and audio drivers:
```
sudo apt-get install pi-bluetooth blueman
sudo apt-get install pulseaudio pavucontrol pulseaudio-module-bluetooth
```


### 2. Install AirPlay on Pi 3



