---
layout: post
title: 'Unrar multiple files'
author: 'Andrew Bounds'
tags:
- linux
---

## Unrar Multiple Files

Not something you run into everyday. However when you need to unrar multiple files in a directory, the following command is handy:

```shell
$ find -type f -name '*.rar' -exec unrar x {} \;
```
