---
layout: post
title: Uninstall LibreOffice on Redhat/Fedora
permalink: uninstall-libreoffice-on-redhatfedora
redirect_from: "2013-09-04-uninstall-libreoffice-on-redhatfedora/"
tags:
- linux
- libreoffice
---

Uninstall any existing installation of LibreOffice.

	$ sudo yum remove libreoffice*

Change directory into the new extracted LibreOffice package.
Install all of the rpms.

	$ sudo rpm -Uvh *.rpm

Change directory to the desktop-integration and install the desktop additions.

	$ cd desktop-integration
	$ sudo rpm -Uvh *freedesktop*.rpm
