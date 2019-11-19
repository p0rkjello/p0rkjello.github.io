---
layout: post
title: 'Detect .NET version using Internet Explorer'
author: 'Andrew Bounds'
tags:
- windows
- dotnet
---

Here is a quick method of getting the .NET versions running on a system.

Past the following text into the Internet Explorer address bar.

    javascript:alert(navigator.userAgent)

![useragent](/assets/img/useragent.png)

__Note:__ Be aware that the userAgent response can be modified.
{: .flash .flash-warn }
