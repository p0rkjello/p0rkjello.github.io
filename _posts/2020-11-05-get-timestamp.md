---
layout: post
title: 'Get-TimeStamp' 
author: 'Andrew Bounds'
tags:
- powershell
- gist
---

Get-TimeStamp function returns a time stamp in string format.

The output is either an ISO 8601 format `2020-11-05T15:49:14Z` useful for logs
or standard yyyyMMddHHmmss `20201105154912` for either logs or output files.

{% gist 518afbc6c4d1e0f50399b4bb392aae79 %}
