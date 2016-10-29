---
layout: post
title: "ARP and L3 switch"
description: ""
categories: 
tags: []
---

If the ARP cache does not contain an entry for the destination IP address, ***the Layer 3 switch broadcasts an ARP request out all its IP interfaces***. The ARP request contains the IP address of the destination. If the device with the IP address is directly attached to the Layer 3 switch, the device sends an ARP response containing its MAC address. The response is a unicast packet addressed directly to the Layer 3 switch. The Layer 3 switch places the information from the ARP response into the ARP cache.
ARP requests contain the IP address and MAC address of the sender, so all devices that receive the request learn the MAC address and IP address of the sender and can update their own ARP caches accordingly.

NOTE: The ARP request broadcast is a MAC broadcast, which means the broadcast goes only to devices that are directly attached to the Layer 3 switch. ***A MAC broadcast is not routed to other networks***. However, some routers, including BrocadeLayer 3 switches, can be configured to reply to ARP requests from one network on behalf of devices on another network.

NOTE: If the router receives an ARP request packet that it is unable to deliver to the final destination because of the ARP timeout and no ARP response is received (the Layer 3 switch knows of no route to the destination address), the router sends an ICMP Host Unreachable message to the source.
