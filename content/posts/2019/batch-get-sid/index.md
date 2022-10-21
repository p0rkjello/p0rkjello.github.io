---
title: Batch script - Get SID
description: Get Windows user/group SID
date: 2019-12-04
author: 'Andrew Bounds'
tags:
- windows
- batch
---

## Get group SID

Get the SID for group "GroupName" and set variable *SID* to that value.

```batch
:: cmd shell

for /f %i in ('wmic group where name^="GroupName" get sid ^| findstr ^S\-d*') do set SID=%i
```

```batch
:: batch file

for /f %%i in ('wmic group where name^="GroupName" get sid ^| findstr ^S\-d*') do set SID=%%i
```

## Get user SID

Get the SID for user "UserName" and set variable *SID* to that value.

```batch
:: cmd shell

for /f %i in ('wmic useraccount where name^="UserName" get sid ^| findstr ^S\-d*') do set SID=%i
```

```batch
:: batch file

for /f %%i in ('wmic useraccount where name^="UserName" get sid ^| findstr ^S\-d*') do set SID=%%i
```

## Notes

The `wmic.exe` command will output a header and value. In the case below *SID* and the actual *SID*. Here is where we use `findstr ^S\-d*` to capture the actual *SID*. The regex looks for a line starting w/ `S` then a `-` followed by a digit `d`.

```batch
C:\> wmic.exe useraccount where name="administrator" get sid
SID
S-1-5-21-259050107-3560852614-2882725377-500
```
