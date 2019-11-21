---
layout: post
title: 'Restart Terminal Services without rebooting'
author: 'Andrew Bounds'
tags:
- windows
---

## Restart Terminal Services Without Rebooting

Microsoft Windows 2003 and lower does not allow you start or stop the Terminal Services service. However if you can't schedule a server reboot there is an alternative.

Download the [Sysinternals tool](http://technet.microsoft.com/en-us/sysinternals) Process Explorer  

Using Process Explorer locate the `svchost.exe -k termsvcs` process. There will be a number of running svchost.exe processes. Hover your mouse over them to identify the correct one. Once you find it, right click and kill it. The Terminal Services service will now be stopped.

![sysinternals-process-explorerer](/assets/img/sysinternals-process-explorerer.jpg)

Restart the service (no longer greyed out) and your good to go.
