---
layout: post
title: 'Fix Shrew Soft VPN on Ubuntu 11.04'
permalink: fix-shrew-soft-vpn-on-ubuntu-11-04
redirect_from: "2011-05-31-fix-shrew-soft-vpn-on-ubuntu-11-04/"
author: 'Andrew Bounds'
tags:
- linux
- ubuntu
- vpn
---

After upgrading my system to Ubuntu 11.04 I was no longer able to establish a VPN connection. [Shrew Soft VPN](http://www.shrew.net/) would time out. It appears that the Reverse Path Filter was causing the issue.

Found the following solution:

Edit your network-security.conf to disable the Reverse Path Filter:

```shell
sudo vim /etc/sysctl.d/10-network-security.conf
```

Set the following items from 1 to 0:

```conf
net.ipv4.conf.default.rp_filter=0
net.ipv4.conf.all.rp_filter=0
```

Save and perform the following command.

```shell
sudo /sbin/sysctl -p
```
