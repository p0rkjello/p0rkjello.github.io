---
layout: post
title: 'Persistent static route via Windows registry'
author: 'Andrew Bounds'
tags: ['windows', 'registry', 'route']
---

For whatever reason I was unable to set a static route via the `route` command.
I assume the system was pending a reboot. However I did not have the time to schedule the outage.

So I added the route directly to the registry. Problem solved, next.

Persistent static routed are stored in the following registry key:
***HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip \Parameters\PersistentRoutes***

Add a new `String Value` with the name `destination,mask,gateway,metric`

### Example

![registry-route](/assets/img/registry-route.png)
