---
title: "Static Routing"
date: 2023-05-06T00:17:07+08:00
draft: true
---

## SSH for MiWiFi

[MiWiFi 开放平台](http://miwifi.com/miwifi_open.html)

> [ssh 登录旧设备的问题解决 - TimTian的文章 - 知乎](https://zhuanlan.zhihu.com/p/30840210)

```sh
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 -oHostKeyAlgorithms=+ssh-dss -oCiphers=+3des-cbc root@your.local.ip.address
```

## Static Routing 

> 静态路由是计算机网络中一种预先配置好的路由选择方法，用于在两个或多个网络之间转发数据包。用一种简单易懂的方式解释静态路由，可以将其看作一张网络地图，上面清晰地标注了到达目的地的路径。
> 
> 在静态路由中，网络管理员需要手动设置每个路由器上的路由表，以确定数据包从一个网络到另一个网络的最佳路径。静态路由的优点在于配置简单且稳定，但是缺点是不适用于大型或动态变化的网络，因为当网络拓扑发生变化时，需要手动更新每个路由器的路由表。
> 
> 与静态路由相对的是动态路由，动态路由使用特定的路由协议，如 OSPF、RIP 或 BGP，来自动计算最佳路径并随网络拓扑变化而更新。动态路由在大型网络中更为实用，因为它们可以自动适应网络变化。然而，动态路由可能需要更多的配置和计算资源，可能在某些情况下不如静态路由简单和高效。

[[AX3600] AX3600和AX1800官方固件连静态路由表都无法设置？](https://www.right.com.cn/forum/thread-6601141-1-1.html)

我非常支持你做的,这样几个地点就实现了虚拟通网.

1. 是的,小米的不支持静态路由.按照技术人员说的,小米的路由器需要上级"商用"路由器来支持静态路由表,然后实现静态路由功能.
2. 小米的路由器是照 OpenWrt 改出来的,所以OpenWrt 有静态路由表功能,小米路由器开发版 SSH 状态下是可以实现静态路由功能的.

ssh 以后

```sh
route add -net 10.168.3.0 netmask 255.255.255.0 gw 192.168.50.29
```

或者
使用vi命令，编辑rc.local。命令是：`vi /etc/rc.local`，并在其中增加一行：

```sh
route add -net 192.168.1.0/24 gw 192.168.13.201
# or 
route add -net 10.168.3.0 netmask 255.255.255.0 gw 192.168.31.203
```

以及

```sh
vi /etc/config/firewall

config defaults
        option syn_flood '0'
        option input 'ACCEPT'
        option output 'ACCEPT'
        option forward 'ACCEPT'     #本来是REJECT
        option drop_invalid '0'       #本来是1
        option disable_ipv6 '0'

config zone
        option name 'lan'
        option network 'lan'
        option input 'ACCEPT'
        option output 'ACCEPT'
        option forward 'ACCEPT'       #本来是REJECT
```

可以实现静态路由功能.
当然首先你要获得SSH,这个可以自己找找.

## Modem

[最新中国移动光猫改桥接方式（中兴ZXHN F663NV9）地域：贵州 适用于动态超密](https://blog.csdn.net/jiguang5432/article/details/123935707)

![Bridge method](https://img-blog.csdnimg.cn/4280028d7c994ea788a483959b03aaf7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAamlndWFuZzU0MzI=,size_20,color_FFFFFF,t_70,g_se,x_16)