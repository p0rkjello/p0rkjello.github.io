---
layout: post
title: Remove ctrl-m characters on Linux
author: 'Andrew Bounds'
support: true
comments: true
tags:
- linux
---

![ctrl-m](/assets/img/ctrl-m.png)

Use `CTRL`+`V` `CTRL`+`M` to type ^M

**vi:**

```text
:%s/^M//g
```

**tr:**

```text
tr -d ^M < file > newfile
```

**sed:**

```text
sed 's/^M//g' file > newfile
```
