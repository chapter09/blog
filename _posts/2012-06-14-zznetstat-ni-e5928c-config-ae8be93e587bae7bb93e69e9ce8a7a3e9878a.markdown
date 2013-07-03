---
author: chapter09
comments: true
date: 2012-06-14 16:26:04+00:00
layout: post
slug: zznetstat-ni-%e5%92%8c-config-a%e8%be%93%e5%87%ba%e7%bb%93%e6%9e%9c%e8%a7%a3%e9%87%8a
title: '[zz]netstat -ni 和 config -a输出结果解释'
wordpress_id: 774
categories:
- Tech
tags:
- Linux
---

caikelun@debian:~$ netstat -ni
Kernel Interface table
Iface   MTU Met   RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
eth0    576 0   3383566      0      0      0  3225169      0      0      0 BMRU
lo    16436 0        70      0      0      0       70      0      0      0 LRU
vmnet  1500 0         0      0      0      0      186      0      0      0 BMRU
vmnet  1500 0     70019      0      0      0    27522      0      0      0 BMRU
Iface 网络接口名称。
MTU
MTU（Maximum Trasmission Unit，最大传输单元）。
链路层具有最大传输单元MTU这个特性，它限制了数据帧的最大长度，不同的网络类型都有一个上限值。以太网的MTU是
1500，你可以用 netstat -i<!-- more -->
命令查看这个值。如果IP层有数据包要传，而且数据包的长度超过了MTU，那么IP层就要对数据包进行分片（fragmentation）操作，使每一片
的长度都小于或等于MTU。我们假设要传输一个UDP数据包，以太网的MTU为1500字节，一般IP首部为20字节，UDP首部为8字节，数据的净荷
（payload）部分预留是1500-20-8=1472字节。如果数据部分大于1472字节，就会出现分片现象。
Met
（Metric，度量值）。（供某些操作系统用，用于计算一条路由的成本）
RX-OK
接收时，正确的数据包数。
RX-ERR
接收时，产生错误的数据包数。
RX-DRP
接收时，丢弃的数据包数。
RX-OVR
接收时，由于过速（在数据传输中，由于接收设备不能接收按照发送速率传送来的数据而使数据丢失）而丢失的数据包数。
TX-OK
发送时，正确的数据包数。
TX-ERR
发送时，产生错误的数据包数。
TX-DRP
发送时，丢弃的数据包数。
TX-OVR
发送时，由于过速而丢失的数据包数。
Flg
标志。
B 已经设置了一个广播地址。
L 该接口是一个回送设备。
M 接收所有数据包（混乱模式）。
N 避免跟踪。
O 在该接口上，禁用ARP。
P 这是一个点到点链接。
R 接口正在运行。
U 接口处于“活动”状态。
caikelun@debian:~$ netstat -nr
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
192.168.124.0   0.0.0.0         255.255.255.0   U         0 0          0 vmnet1
192.168.28.0    0.0.0.0         255.255.255.0   U         0 0          0 vmnet8
58.25.72.0      0.0.0.0         255.255.252.0   U         0 0          0 eth0
0.0.0.0         58.25.72.1      0.0.0.0         UG        0 0          0 eth0
Destination
目标网络或主机。
Gateway
网关。如果没有使用网关，就会出现一个星号（*）或者0.0.0.0。
Genmask
路由的网络掩码。内核在将信息包的 IP 地址与路由的目的地 IP 地址进行比较之前，将 Genmask 值与信息包的 IP 地址逐位进行“与”操作，从而使路由“通用化”。
Flags
标志。
U 路由是可用的。
H 目标是一台主机。
G 路由采用网关。
R 动态路由复原。
D 路由表的条目是由ICMP重定向消息生成的。
M 路由表条目已被ICMP重定向消息修改。
A 被addrconf安装。
C 缓存记录。
! 拒绝路由。
MSS
MSS（Maximum Segment Size，最大分段尺寸），也是内核所构建以通过该路由发送的数据报的最大尺寸。
Window
系统一次从远程主机接收突发的最大量数据。
irtt
irtt（initial round trip tim，初始往返时间）。TCP 协议确保主机间可靠地发送数据，如果数据已经丢失，则重新发送。TCP
协议一直对发送给远程端点的数据报和接收到的确认所花费的时间进行记数，以便知道假定要重发数据报前需要等待的时间；这个过程称为往返时间。TCP
协议将使用第一次建立连接时所用时间作为初始往返时间的值。对于大多数类型的网络，用缺省值就够了，但对某些速度较慢的网络（特别是某些业余的分组无线网
络），这个时间太短了，会造成不必要的重发。可以使用 route 命令设置 irtt
值。在上面这个路由表中，这些字段均为零值，这表明正在使用缺省值。
Iface
路由使用的网络接口名称。
caikelun@debian:~$ sudo ifconfig eth0
eth0      Link encap:Ethernet  HWaddr 00:1A:4D:45:AF:12
inet addr:58.25.73.202  Bcast:255.255.255.255  Mask:255.255.252.0
UP BROADCAST RUNNING MULTICAST  MTU:576  Metric:1
RX packets:1562525 errors:0 dropped:0 overruns:0 frame:0
TX packets:1393140 errors:0 dropped:0 overruns:0 carrier:0
collisions:0 txqueuelen:1000
RX bytes:611386679 (583.0 MiB)  TX bytes:230613477 (219.9 MiB)
Interrupt:169 Base address:0xc000
Link encap
接口的概要描述。
HWaddr
网卡的硬件地址。
inet addr
网卡的IP地址。
Bcast
广播地址。
Mask
网络掩码。
UP
表示“接口已启用”。
BROADCAST
表示“主机支持广播”。
RUNNING
表示“接口在工作中”。
MULTICAST
表示“主机支持多播”。
MTU
见上上表。
Metric 见上上表。（同“Met”）
RX packets 接收时，正确的数据包数。
RX errors 接收时，产生错误的数据包数。
RX dropped 接收时，丢弃的数据包数。
RX overruns 接收时，由于过速而丢失的数据包数。
RX frame 接收时，发生frame错误而丢失的数据包数。
(以太网是一种共享媒体(shared medium)，所以必须要有机制来决定由谁来使用传输媒体，在以太网中所采用的是CSMA/CD（Carrier Sense Multiple Access with Collision Detection）方式，步骤如下：
1 将要传输的数据切割成Frame，作为传输单位。
2 要传输时先侦测电缆上是否有设备送Frame(Carrier Sense)。
3 若沒有设备使用，才准备发送Frame，并侦测是否有另外的设备发送Frame(Collision Detection)。
4 若发生碰撞，则各自等待一段随机的时间，再重试( Backoff Algorithm)。
TX packets 发送时，正确的数据包数。
TX errors  发送时，产生错误的数据包数。
TX dropped  发送时，丢弃的数据包数。
TX overruns  发送时，由于过速而丢失的数据包数。
TX carrier  发送时，发生carrier错误而丢失的数据包数。 collisions 冲突信息包的数目。 txqueuelen 发送队列的大小。 RX bytes 接收的数据量。
TX bytes 发送的数据量。 Interrupt IRQ 中断地址。 Base address 基址。
