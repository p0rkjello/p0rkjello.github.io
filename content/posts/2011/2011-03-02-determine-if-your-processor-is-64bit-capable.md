---
layout: post
title: 'Determine if your processor is 64bit capable'
author: 'Andrew Bounds'
support: true
comments: true
tags:
- linux
- cli
---

## Determine if your processor is 64bit capable

Grep the output of `/proc/cpuinfo` for the lm (long mode) flag.

```shell
grep lm /proc/cpuinfo

flags       : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe nx lm constant_tsc arch_perfmon pebs bts aperfmperf pni dtes64 monitor ds_cpl vmx smx est tm2 ssse3 cx16 xtpr pdcm sse4_1 xsave lahf_lm ida dts tpr_shadow vnmi flexpriority
flags       : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe nx lm constant_tsc arch_perfmon pebs bts aperfmperf pni dtes64 monitor ds_cpl vmx smx est tm2 ssse3 cx16 xtpr pdcm sse4_1 xsave lahf_lm ida dts tpr_shadow vnmi flexpriority
```

[Long Mode (lm) description via Wikipedia:](http://en.wikipedia.org/wiki/Long_mode)

>In the x86-64 computer architecture, long mode is the mode where a 64-bit application (or operating system) can access the 64-bit instructions and registers. 32-bit programs and 16-bit protected mode programs are executed in a compatibility sub-mode; real mode or virtual 8086 mode programs cannot run in this mode.
