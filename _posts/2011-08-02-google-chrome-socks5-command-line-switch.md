---
layout: post
title: Chrome socks5 command line switch
permalink: google-chrome-socks5-command-line-switch
redirect_from: "2011-08-02-google-chrome-socks5-command-line-switch/"
tags:
- chrome
- proxy
- socks5
---

In Windows the Chrome browser uses your system wide proxy settings. This can be a problem if you have a need to run different browsers and/or applications. An example would be running a Secure Shell proxy to get through a content filtering firewall.

To get around the Chrome system wide proxy settings, use a command line switch. Open or create a shortcut to [Chrome](http://www.google.com/chrome/)/[Chromium](http://www.chromium.org/Home)

![chrome-socks5-cmd](/assets/img/chrome-socks5-proxy-command-line-switch.jpg)

    --proxy-server="socks5://127.0.0.1:8080"

Replace 127.0.0.1 with your proxy address and 8080 with your proxy port.
