---
title: "Save readonly file in vim"
description: "Save a file in vim when opened readonly"
date: 2019-10-04
tags: 
- vim
- linux
---

## vim [readonly]

Forget to run `sudo` when opening the file and met with the following error?

```text
E45: 'readonly' option is set (add ! to override)
```

No worries, the following command will allow you to save the file without losing your edits.

```bash
:w !sudo tee %
```

`:w` pipe the contents of the buffer through command `!sudo tee` to `%` the current file.
