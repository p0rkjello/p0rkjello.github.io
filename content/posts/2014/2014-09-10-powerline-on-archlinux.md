---
layout: post
title: Powerline on Archlinux
permalink: powerline-on-archlinux
redirect_from: "2014-09-10-powerline-on-archlinux/"
support: true
comments: true
tags:
- linux
- archlinux
- powerline
- tmux
---

## Powerline on Archlinux

I recently switched from GNU Screen to Tmux. While reviewing random tmux.confs on Github I stumbled onto the screenshot below of tmux & powerline.

> [Powerline](https://powerline.readthedocs.org/en/latest/overview.html) is a statusline plugin for vim, and provides statuslines and prompts for several other applications, including zsh, bash, tmux, IPython, Awesome and Qtile.

[https://github.com/gpakosz/.tmux](https://github.com/gpakosz/.tmux)

![powerline-tmux](/assets/img/powerline-tmux.png)

An [AUR package](https://aur.archlinux.org/packages/python-powerline-git/) available for `Powerline`. However for this document I am going to detail the standard installation.

## Powerline Installation

Install python-pip (and if you don't aleady have it, git):

```shell
$ sudo pacman -Sy python-pip git
```

Install Powerline:

```shell
$ su -c 'pip install git+git://github.com/Lokaltog/powerline'
```

Powerline is installed into:
`/usr/lib/python3.4/site-packages/powerline`

Powerline scripts for bash, tmux, fish etc are located in:
`/usr/lib/python3.4/site-packages/powerline/bindings`

Install the Powerline patched fonts:

```shell
$ wget https://github.com/Lokaltog/powerline/raw/develop/font/PowerlineSymbols.otf https://github.com/Lokaltog/powerline/raw/develop/font/10-powerline-symbols.conf
$ sudo mv PowerlineSymbols.otf /usr/share/fonts/
$ sudo fc-cache -vf
$ sudo mv 10-powerline-symbols.conf /etc/fonts/conf.d/
```

## Dotfile Configuration

Edit your various dotfiles to run their powershell configuration scripts.

### ~/.tmux.conf

```conf
source /usr/lib/python3.4/site-packages/powerline/bindings/tmux/powerline.conf
```

### ~/.zshrc

```shell
if [[ -r /usr/lib/python3.4/site-packages/powerline/bindings/zsh/powerline.zsh ]]; then
  source /usr/lib/python3.4/site-packages/powerline/bindings/zsh/powerline.zsh
fi
```

### ~/.bashrc

```shell
if [ -f /usr/lib/python3.4/site-packages/powerline/bindings/bash/powerline.sh ]; then
  source /usr/lib/python3.4/site-packages/powerline/bindings/bash/powerline.sh
fi
```

Patched fonts are required on remote systems used to connect to your powerline/tmux box. Download and install a patched font for your system. [https://github.com/Lokaltog/powerline-fonts](https://github.com/Lokaltog/powerline-fonts)

![powerline](/assets/img/powerline.png)

## Failure / Nothing works!

Should nothing above work as expected and/or you give up.

**Uninstall Powerline:**

```shell
$ su -c 'pip uninstall powerline'
```
