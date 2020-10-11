---
title: Windows 下多网卡路由表设置
date: 2020-10-11 08:53:00
categories: Research
toc: true
tags:
  - Windows
  - Routing
  - NetMask
---

经常遇到这样的情形：办公网与外网不通，办公时如需连外网查资料，不得不来回切换网络或通过其他设备上外网再回到办公电脑前，来回折腾苦不堪言。自然产生需求：同一台设备如何同时连接办公内网及外网？

<!-- more -->

## 实验配置

- 内网端：办公网口 / 可分享网络的办公用电脑
- 外网端：连通外网的路由器 / 热点 / 移动设备 USB 分享 / 其他可连至外网的设备或信号
- 多网卡计算机

其中**内网端**可为办公网口，通过网线接入目标笔记本；但一般情况下公司会提供办公专用笔记本，通过网络共享技术，可将办公网络共享给通过网线与之相连的设备。**外网端**顾名思义，可使目标笔记本上外网的设备或手段。多网卡设备中的「多网卡」可以认为是以太网（Ethernet）及无线网（Wi-Fi）或其他计算机中的网络设备。

## 实验场景一：个人电脑通过办公电脑上公司内网

> 首先从简单的场景实验开始：办公电脑 A 通过 Wi-Fi 连接公司内网，并通过网线与个人电脑 B 相连。

将办公电脑 A 中网络适配器设置的 Wi-Fi 适配器（设备名可能不同）共享打开：属性 - 共享 - 勾选允许网络用户通过该电脑连接至网络。此时电脑 A 的以太网适配器会将 IP 设置为 `192.168.137.1`。

在电脑 B 中的以太网设置中：

- 自动获取 IP 地址 / DNS 服务器地址
- 如需固定 IP 地址，则需将默认网关及 DNS 服务器设置为 `192.168.137.1`

## 实验场景二：设置个人电脑内网外网均可访问

> 有了实验一的经验，可否简单地将外网网线接入个人电脑，并成功访问外网及内网？

此时打开个人电脑 Wi-Fi，发现外网可访问，但实验一中的内网网址无法访问了。**管理员**权限下在 `cmd` 中输入

```Bat
route print -4
```

可获取路由表信息（IPv4）：

```
===========================================================================
Interface List
 15...xx xx xx xx xx xx ......Realtek PCIe GbE Family Controller
 32...xx xx xx xx xx xx ......Microsoft Wi-Fi Direct Virtual Adapter
 16...xx xx xx xx xx xx ......Realtek 8821CE Wireless LAN 802.11ac PCI-E NIC
  1...........................Software Loopback Interface 1
===========================================================================

IPv4 Route Table
===========================================================================
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0    192.168.137.1  192.168.137.101    281
          0.0.0.0          0.0.0.0      192.168.8.1    192.168.8.101     50
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
      192.168.8.0    255.255.255.0         On-link     192.168.8.101    306
    192.168.8.101  255.255.255.255         On-link     192.168.8.101    306
    192.168.8.255  255.255.255.255         On-link     192.168.8.101    306
    192.168.137.0    255.255.255.0         On-link   192.168.137.101    281
  192.168.137.101  255.255.255.255         On-link   192.168.137.101    281
  192.168.137.255  255.255.255.255         On-link   192.168.137.101    281
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link     192.168.8.101    306
        224.0.0.0        240.0.0.0         On-link   192.168.137.101    281
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link     192.168.8.101    306
  255.255.255.255  255.255.255.255         On-link   192.168.137.101    281
===========================================================================
Persistent Routes:
  Network Address          Netmask  Gateway Address  Metric
          0.0.0.0          0.0.0.0    192.168.137.1  Default
===========================================================================
```

可以看到针对目的网段 `0.0.0.0`，活动路由中存在 2 条路由设定。Windows 下默认网关是指发往 `0.0.0.0` 缺省网段的地址，其目的地为所有未知路由的网段，所以设置了 2 个默认网关之后，其中的一个网关将被启用，而另外一个将作为冗余。所以在浏览器中试图访问内网时，例如访问 IP 为 `10.30.10.101`，系统在 2 条路由中挑选一条进行访问，系统会偏向跳数（Metric）较小的路由进行访问，在测试中的情况即为网关为外网的这条路由，导致内网无法访问。

**解决方案**

- 针对 `0.0.0.0` 网段进行针对性设置，如默认网段走外网 `192.168.8.1`，而内网网段走内网 `192.168.137.1`
- 针对内网网段，设定网关为内网 `192.168.137.1`

例如，公司内网网段为 `10.30.*.*`，首先删除路由为 `0.0.0.0` 的 2 条记录，并添加默认网段的路由网关为 `192.168.8.1`，内网网段的路由网关为 `192.168.137.1`。需要注意的是命令中的**接口号**是否与实际情况相对应。

```Bat
route delete          0.0.0.0
route add -p          0.0.0.0  mask        0.0.0.0      192.168.8.1   IF 16
route add -p        10.30.0.0  mask    255.255.0.0    192.168.137.1   IF 15
route add -p    192.168.137.0  mask  255.255.255.0    192.168.137.1   IF 15
```

设定后的路由表：

```
IPv4 Route Table
===========================================================================
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0      192.168.8.1    192.168.8.101     51
        10.30.0.0      255.255.0.0    192.168.137.1  192.168.137.101     26
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
      192.168.8.0    255.255.255.0         On-link     192.168.8.101    306
    192.168.8.101  255.255.255.255         On-link     192.168.8.101    306
    192.168.8.255  255.255.255.255         On-link     192.168.8.101    306
    192.168.137.0    255.255.255.0    192.168.137.1  192.168.137.101     26
  192.168.137.101  255.255.255.255         On-link   192.168.137.101    281
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link     192.168.8.101    306
        224.0.0.0        240.0.0.0         On-link   192.168.137.101    281
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link     192.168.8.101    306
  255.255.255.255  255.255.255.255         On-link   192.168.137.101    281
===========================================================================
Persistent Routes:
  Network Address          Netmask  Gateway Address  Metric
    192.168.137.0    255.255.255.0    192.168.137.1       1
        10.30.0.0      255.255.0.0    192.168.137.1       1
          0.0.0.0          0.0.0.0      192.168.8.1       1
```

## GUI 程序设定路由表

> [NetRouteView - Network Route Utility for Windows][NetRouteView]

[NetRouteView]: https://www.nirsoft.net/utils/network_route_view.html
