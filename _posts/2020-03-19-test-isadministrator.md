---
layout: post
title: 'Test-IsAdministrator' 
author: 'Andrew Bounds'
tags:
- powershell
- gist
---

Test-IsAdministrator is a PowerShell function to determine if the current user is an administrator.

`[Security.Principal.WindowsBuiltinRole]::Administrator` returns the `WindowsBuiltInRole` for the Administrator.

`GetCurrent()` returns a WindowsIdentity object that represents the current Windows user.

`WindowsPrincipal.IsInRole` Method determines whether the current principal belongs to the Windows user group specified.

{% gist 8638d1560281e5f0695bfb47984343cf %}
