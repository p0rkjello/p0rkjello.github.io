---
title: "Get-TimeStamp"
date: 2020-11-05
tags:
- powershell
---

Get-TimeStamp function returns a time stamp in string format.

The output is either an ISO 8601 format `2020-11-05T15:49:14Z` useful for logs
or standard yyyyMMddHHmmss `20201105154912` for either logs or output files.

{{< code language="powershell" title="Get-TimeStamp" >}}
function Get-TimeStamp {
    param (
        # Return an UTC ISO 8601 timestamp (ie: "2020-06-16T17:33:27Z")
        [switch]
        $iso8601 = $false
    )
    $String = switch ($iso8601.IsPresent) {
        true { 'yyyy-MM-ddTHH:mm:ssZ' }
        false { 'yyyyMMddHHmmss' }
    }
    (Get-Date).ToUniversalTime().ToString($String)
}
{{< /code >}}
