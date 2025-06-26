---
dg-publish: true
---

# some applications and tools

## basic tools

### media

#### mpd

- mpd
- frontend for mpd:
    - cli:mpc
    - tui:
        - ncmpc
        - ncmpcpp
        - mmtc (no, weird config)
        - rmpc

`easytag` to edit metadata of a audio file.

#### mpv

video player and more

#### ffmpeg

so powerful

### system

#### systemd-manager-tui

[systemd-manager-tui](https://crates.io/crates/systemd-manager-tui)

#### aconfmgr

A configuration manager for Arch Linux. Kind of like nixOS. [github](https://github.com/CyberShadow/aconfmgr)

## DE

### wm

#### dwm

suckless

#### niri

page scroll

#### i3

tiling

#### other

[cwc](https://cudiph.github.io/cwc/apidoc/documentation/00-getting-started.md.html)

[github](https://github.com/Cudiph/cwcwm)

### utils

- xorg-xeye: help to find xwayland app while running wayland
- xev wev: key stroke under X and wayland

### debug

[Xephyr](https://wiki.archlinux.org/title/Xephyr) is a nested X server that runs as an X application.

If you wish to run a nested X window, you will need to specify a new display:

```
$ Xephyr -br -ac -noreset -screen 800x600 :1
```

This will launch a new Xephyr window with a DISPLAY of ":1". In order to launch an application in that window, you would need to specify that display:

```
$ DISPLAY=:1 xterm
```

If you want to launch a specific WM, [spectrwm](https://wiki.archlinux.org/title/Spectrwm "Spectrwm") for example, you would type:

```
$ DISPLAY=:1 spectrwm
```

You can also launch Xephyr with your [xinitrc](https://wiki.archlinux.org/title/Xinitrc "Xinitrc") using startx:

```
$ startx -- /usr/bin/Xephyr :1
```

Grabbing and un-grabbing user input: Pressing `Ctrl+Shift` should lock/unlock your mouse pointer and your keystrokes inside Xephyr window exclusively if possible.  
If using KDE, create a window rule to ignore global shortcuts. Then you can use `Alt+Tab` inside Xephyr.

Tips and tricks: Other examples for situations where Xephyr can be useful are:

1. A testing environment for an X application, or feature, in which the tester would like to keep working in their usual X environment, yet defending the other applications from failures of the application under test.
2. [OpenSSH#Remote](https://wiki.archlinux.org/title/OpenSSH#Remote "OpenSSH") emphasize 3 settings in the sshd server configuration file for [OpenSSH#X11 forwarding](https://wiki.archlinux.org/title/OpenSSH#X11_forwarding "OpenSSH") (over ssh). 2 of these, out of 3, are the default settings. When the ssh client can not influence the ssh server administrator to set the 3rd one, `X11Forwarding`, to yes, [Forwarding X11 over ssh](https://unix.stackexchange.com/questions/12777/forwarding-x11-over-ssh-if-the-server-configuration-doesnt-allow-it) uses Xephyr as a work around to be installed in the ssh client machine.

## workflow

### task and event

- dstask
- todo.txt
- timewarrior
- taskwarrior
    - tasknc
    - taskwarriortui
- todoman

#### calendar

- vdirsyncer
- khal

### email

#### aerc

###url

### vcs

#### avc

[AVC](https://github.com/assembler-0/AVC)

Achieve version control

## useful

### not now, or not me

#### dracut

Alter for mkinitcpio

#### limine

[limine](https://wiki.archlinux.org/title/Limine)
Alter for grub

### ninve

[ninve](https://github.com/Niedzwiedzw/ninve) A tui video editor. Use `mpv` to play and `ffmpeg` to edit.

not support fo timeshift btrfs snap yet

### I'll back here

#### httrack

#### whois

```tldr
  Command-line client for the WHOIS (RFC 3912) protocol.
  More information: <https://manned.org/whois>.

  Get information about a domain name:

      whois example.com

  Get information about an IP address:

      whois 8.8.8.8

  Get abuse contact for an IP address:

      whois -b 8.8.8.8
```

#### jujustu

[jj](https://github.com/jj-vcs/jj)

#### gitoxide

[github](https://github.com/GitoxideLabs/gitoxide)
An idiomatic, lean, fast & safe pure Rust implementation of Git

#### [yabsnap](https://wiki.archlinux.org/title/Yabsnap)

another `snapper`

not support fo timeshift btrfs snap yet

## Interesting

### useful

#### clivm

[clivm](https://github.com/AruAVI/clivm-git) is a lightweight tool to locally create containers for multiple Linux distributions.

```PKGBUILD
# Maintainer: AruAVI <arubaanimates@gmail.com>

pkgname=clivm
pkgver=1.0.0
pkgrel=1
pkgdesc="CLI-based Linux virtualization management tool"
arch=('any')
url="https://github.com/AruAVI/clivm"
license=('MIT')
depends=('debootstrap' 'arch-install-scripts' 'wget' 'git')
makedepends=()
source=("clivm-${pkgver}.tar.gz")
sha256sums=('bfe2e60f517b75d5006ceabde7fa24e4c72460a132d99674da836bef48794b61')

build() {
  # Create the subdirectory and move all extracted files there
  mkdir -p "$srcdir/clivm-1.0.0"
  mv "$srcdir"/* "$srcdir/clivm-1.0.0"/ 2>/dev/null || true
}

package() {
  cd "$srcdir/clivm-1.0.0"

  # Create directories in package
  install -dm755 "$pkgdir/usr/share/clivm/binaries"
  install -dm755 "$pkgdir/usr/share/clivm/installers"

  # Copy files into package
  cp -r binaries/* "$pkgdir/usr/share/clivm/binaries/"
  cp -r installers/* "$pkgdir/usr/share/clivm/installers/"

  # Install executable launcher
  install -Dm755 clivm.py "$pkgdir/usr/bin/clivm"
}
```

#### [yutto](https://yutto.nyakku.moe/)

a bilibili downloader

### not now, or not me

#### stew-bin

install binary from github

#### bilibili shadow replay

[doc](https://bsr.xinrea.cn/)

### not much useful or unnecessary

#### gowall

A tool to convert a wallpaper's colorscheme, like nord or onedark

#### [quarkdown](https://github.com/iamgio/quarkdown)

Turn markdown with additional marks to pdf or html, like LaTeX.

![paper](https://raw.githubusercontent.com/iamgio/quarkdown/project-files/images/code-paper.png)

![chart](https://raw.githubusercontent.com/iamgio/quarkdown/project-files/images/code-chart.png)

#### [fancy-cat](https://github.com/freref/fancy-cat)

cat pdf or other

#### ripgrep-all

`rga` to match in pdf

#### lsix

list pictures by sixel

#### chafa

show pixels of pictures through sixel

#### btrfs-heatmap

btrfs usage by hilbert curve

#### gping

render in tui for ping

#### mpvpaper

wallpaper by mpv in wayland

#### [codesnap](https://codesnap.dev/)

generate pictures of code snaps

### wtf Interesting

#### activate-linux

a watermark
