---
layout: post
title: Remove ctrl-m characters on Linux
permalink: remove-ctrl-m-characters-linux
redirect_from: "2013-11-24-remove-ctrl-m-characters-linux/"
tags:
- linux
---

![ctrl-m](/content/img/ctrl-m.png)

Use `CTRL`+`V` `CTRL`+`M` to type ^M

**vi:**

	:%s/^M//g

**tr:**

	tr -d ^M < file > newfile

**sed:**

    sed 's/^M//g' file > newfile
