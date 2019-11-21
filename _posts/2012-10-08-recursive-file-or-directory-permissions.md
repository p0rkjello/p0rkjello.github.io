---
layout: post
title: Set recursive file or directory permission
permalink: recursive-file-or-directory-permissions
redirect_from: "2012-10-08-recursive-file-or-directory-permissions/"
tags:
- linux
- bash
---

Command to recursively change either file or directory permissions in Bash.

Example of changing directory permissions to 755

```shell
sudo find . -type d -exec chmod 755 {} \;
```

Example of changing file permissions to 666

```shell
sudo find . -type f -exec chmod 666 {} \;
```
