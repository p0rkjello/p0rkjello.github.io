---
layout: post
title: 'Citrix ICA not listening on port 1494'
permalink: citrix-ica-not-listening-on-port-1494
redirect_from: '2012-04-24-citrix-ica-not-listening-on-port-1494/'
author: 'Andrew Bounds'
support: true
comments: true
tags:
- citrix
---

I ran into an issue with some Citrix servers in the environment. Users were getting a protocol driver error. After poking around I noticed ICA was not listening on port 1494.

It appears that this issue was caused by Microsoft patch [MS12-024](http://support.microsoft.com/kb/2653956).

> Known issues with this security update
> After you install this security update on a Windows Server 2003 SP2-based terminal server, Remote Desktop Protocol (RDP) clients or Independent Computing Architecture (ICA) clients may be unable to connect to the terminal server. To resolve this problem, install hotfix 958476.
> MS Hotfix 958476 is available [here](http://support.microsoft.com/kb/958476).

The following instructions correct the issue until the hotfix can be applied:

Identify that ICA is not listening on port 1494.

    netstat -an | findstr 1494

Open the Terminal Services MMC

    tscc.msc

Disable then Enable ICA

Run netstat again to verify ICA is listening on port 1494.

Retest the Citrix connection.
