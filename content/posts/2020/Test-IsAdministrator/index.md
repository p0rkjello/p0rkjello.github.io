---
title: "Test-IsAdministrator" 
date: 2020-03-19
description: "PowerShell function to test if the current user is an administrator."
tags:
- powershell
---

Test-IsAdministrator is a PowerShell function to determine if the current user is an administrator.

{{< code language="powershell" title="Test-IsAdministrator" >}}
function Test-IsAdministrator {
    $Administrator = [Security.Principal.WindowsBuiltinRole]::Administrator
    $User = [Security.Principal.WindowsIdentity]::GetCurrent()
    ([Security.Principal.WindowsPrincipal]($User)).IsInRole($Administrator)
}
{{< /code >}}

`[Security.Principal.WindowsBuiltinRole]::Administrator` returns the `WindowsBuiltInRole` for the Administrator.

`GetCurrent()` returns a WindowsIdentity object that represents the current Windows user.

`WindowsPrincipal.IsInRole` Method determines whether the current principal belongs to the Windows user group specified.