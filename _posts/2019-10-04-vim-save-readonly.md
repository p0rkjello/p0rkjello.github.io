---
layout: post
title: Save readonly file in vim
tags: [vim, linux, oneliner]
---

## vim [readonly]

Forgot to run `sudo` when opening the file and met with the following error?

```
E45: 'readonly' option is set (add ! to override)
```

No worries, the following command will allow you to save the file without losing your edits.

```bash
:w !sudo tee %
```

`:w` pipe the contents of the buffer through command `!sudo tee` to `%` the current file.
