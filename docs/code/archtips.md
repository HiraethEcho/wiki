---
title: Tips for archlinux 
toc: true
tags:
  - arch
date: 2024-11-18
---

## sh
后台挂起
```sh
nohup onedrivegui & > /dev/null
```

## configuration
**net**
Using iwd as backend of NetworkManager:

/etc/NetworkManager/NetworkManager.conf :
```
[device]
wifi.backend=iwd
```
then
```sh
systemctl mask wpa_supplicant
systemctl enable iwd
```

**sound**

- ALSA: is a set of built-in Linux kernel modules.
- PulseAudio: is a general purpose sound server intended to run as a middleware between your applications and your hardware devices, either using ALSA or OSS.
- pamixer: cli mixer of PulseAudio
- pavucontrol: gui of PulseAudio

```
sudo pacman -S alsa-ultis pulseaudio pavucontrol
pulseaudio --check
pulseaudio -D
```

**light**
```sh

sudo pacman -S acpilight
sudo gpasswd video -a _username_ # 或者
sudo usermod -aG video _username_
```

**keyboard**:

find id of touchpad:
```
xinput list | grep -i "Touchpad" | awk '{print $6}' | sed 's/[^0-9]//g'
```

config keys, <kbd>caps</kbd> as <kbd>escape</kbd> and <kbd>ctrl</kbd>
```sh
setxkbmap -option ctrl:nocaps &
xcape -e 'Control_L=Return' &
xcape -e 'Alt_L=Escape' &
```


### wifi

pacman -S grub efibootmgr vim iwd dhcpcd sudo networkmanager

systemctl enable dhcpcd NetworkManager iwd

### pacman etc

`sudo vim /etc/pacman.d/mirrorlist`

```
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
```

```sh
sudo pacman -S archlinux-keyring
sudo pacman -Syyu
```

`sudo vim /etc/pacman.conf`, add

```
[archlinuxcn]
SigLevel = Optional TrustAll
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

```sh
sudo pacman -S archlinuxcn-keyring
```

clean pacman:

```sh
paccache -r # 清理缓存,仅包含最近的三个版本
paccache -rk1 # 清理缓存,仅包含最近的1个版本
pacman -Sc # 清理未安装软件包
pacman -Scc # 清理缓存中所有内容
sudo pacman -Rcsn $(pacman -Qdtq -)
journalctl --vacuum-size=50M #限制日志
```

