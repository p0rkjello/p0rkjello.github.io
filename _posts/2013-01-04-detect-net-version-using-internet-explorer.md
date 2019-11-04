---
layout: post
title: Detect .NET version using Internet Explorer
permalink: detect-net-version-using-internet-explorer
redirect_from: "2013-01-04-detect-net-version-using-internet-explorer/"
tags:
- windows
- .net
---

Here is a quick method of getting the .NET versions running on a system.

Past the following text into the Internet Explorer address bar.

    javascript:alert(navigator.userAgent)

![useragent](/content/img/useragent.png)

***Note:*** Be aware that the userAgent response can be modified.
