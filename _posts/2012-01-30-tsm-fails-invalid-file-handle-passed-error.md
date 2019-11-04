---
layout: post
title: TSM Invalid file handle passed
permalink: tsm-fails-invalid-file-handle-passed-error
redirect_from: "2012-01-30-tsm-fails-invalid-file-handle-passed-error/"
tags:
- windows
- tsm
---

If TSM client job failed with and error of **"Invalid file handle passed"**. Here is a quick solution.

Stop the TSM Client Scheduler service.

	net stop "TSM Client Scheduler"

Delete (or rename) the `adsm.sys` directory. It will be recreated by TSM.

![adsm-sys](/content/img/adsm-sys.png)

Restart the TSM Client Scheduler service.

	net start "TSM Client Scheduler"

Rerun the backup job.
