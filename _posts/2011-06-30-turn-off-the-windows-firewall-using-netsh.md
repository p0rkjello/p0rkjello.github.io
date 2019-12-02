---
layout: post
title: Turn off Windows firewall using netsh
permalink: turn-off-the-windows-firewall-using-netsh
redirect_from: "2011-06-30-turn-off-the-windows-firewall-using-netsh/"
author: 'Andrew Bounds'
support: true
comments: true
tags:
- windows
- netsh
---

Often the first step in troubleshooting communication issues is to disable the Windows firewall. Below is the netsh command.

```console
netsh advfirewall set allprofiles state off
```

Additional netsh.exe information:
[Windows Server® 2008 Network Shell (Netsh) Technical Reference](http://www.microsoft.com/download/en/details.aspx?displaylang=en&amp;id=4702)
