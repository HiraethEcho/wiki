---
title: "Xephyr - ArchWiki"
source: "https://wiki.archlinux.org/title/Xephyr"
author:
date: "2025-05-17T20:44:56+08:00"
description:
dg-publish: "true"
---
**Xephyr** is a nested X server that runs as an X application.

This may be useful to workaround a badly written application. For example, Supermicro servers may be controlled with a java ipmi kvm viewer application. While the server is rebooting, the application frequently recreates its window and steals focus from your current window. This happens several times per minute, and makes your work impossible. It is not obvious how to make a window rule that prevents such application's window from gaining focus when created, because focus must be given when launched for the first time. Using Xephyr allows you to keep these window recreations inside a separate window, which does not steal focus from your currently opened window(s).

## Installation

[Install](https://wiki.archlinux.org/title/Install "Install") [xorg-server-xephyr](https://archlinux.org/packages/?name=xorg-server-xephyr).

## Usage

If you wish to run a nested X window, you will need to specify a new display:

```
$ Xephyr -br -ac -noreset -screen 800x600 :1
```

This will launch a new Xephyr window with a DISPLAY of ":1". In order to launch an application in that window, you would need to specify that display:

```
$ DISPLAY=:1 xterm
```

### Launching window managers

If you want to launch a specific WM, [spectrwm](https://wiki.archlinux.org/title/Spectrwm "Spectrwm") for example, you would type:

```
$ DISPLAY=:1 spectrwm
```

You can also launch Xephyr with your [xinitrc](https://wiki.archlinux.org/title/Xinitrc "Xinitrc") using startx:

```
$ startx -- /usr/bin/Xephyr :1
```

### Grabbing and un-grabbing user input

Pressing `Ctrl+Shift` should lock/unlock your mouse pointer and your keystrokes inside Xephyr window exclusively if possible.

### Sending Alt+Tab

If using KDE, create a window rule to ignore global shortcuts. Then you can use `Alt+Tab` inside Xephyr.

## Tips and tricks

Other examples for situations where Xephyr can be useful are:

1. A testing environment for an X application, or feature, in which the tester would like to keep working in their usual X environment, yet defending the other applications from failures of the application under test.
2. [OpenSSH#Remote](https://wiki.archlinux.org/title/OpenSSH#Remote "OpenSSH") emphasize 3 settings in the sshd server configuration file for [OpenSSH#X11 forwarding](https://wiki.archlinux.org/title/OpenSSH#X11_forwarding "OpenSSH") (over ssh). 2 of these, out of 3, are the default settings. When the ssh client can not influence the ssh server administrator to set the 3rd one, `X11Forwarding`, to yes, [Forwarding X11 over ssh](https://unix.stackexchange.com/questions/12777/forwarding-x11-over-ssh-if-the-server-configuration-doesnt-allow-it) uses Xephyr as a work around to be installed in the ssh client machine.