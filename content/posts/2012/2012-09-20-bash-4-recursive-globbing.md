---
layout: post
title: 'Bash 4 recursive globbing'
author: 'Andrew Bounds'
support: true
comments: true
tags:
- linux
- bash
---

Bash version 4 introduced a new option called globstar. When the option is set, recursive globbing with ** is enabled.

Lets say you want to move all files from a directory and its subdirectories. Most common method would be to use the `find` command.

For example, find and move all .mp3 files from the current and all subdirectories to a directory named music.

```shell
$ sudo find . -type f -exec mv *.mp3 /music {} \;
```

The can be simplified by enabling the Bash option globstar.

```shell
$ shopt -s globstar
```

```shell
$ shopt |grep globstar
globstar        on
```

With the BASH globstar setting enabled the command is as simple as:

```shell
$ mv **/*.mp3 /music
```
