---
title: "ArtixOpenRC"
source: "https://www.cnblogs.com/wonux/p/9760678.html"
author:
date: "2025-05-15T12:50:49+08:00"
description: '轻量桌面Archlinux用户逃离systemd,拥抱Gentoo的openrc. 镜像源:官方镜像源非常慢，曾经一度体验artix后就放弃了，后来发现了清华和腾讯云的镜像，速度非常快，现在又重新安装了Artix，替代Arch和Manjaro成为了使用的主力发行版。 Artix介绍: "A'
dg-publish: true
---

# 轻量桌面Archlinux用户逃离systemd,拥抱Gentoo的openrc.

- 镜像源:官方镜像源非常慢，曾经一度体验artix后就放弃了，后来发现了清华和腾讯云的镜像，速度非常快，现在又重新安装了Artix，替代Arch和Manjaro成为了使用的主力发行版。

## Artix介绍:

[Artix Linux on DistroWatch](https://distrowatch.com/table.php?distribution=artix):  
Artix Linux is a fork (or continuation as an autonomous project) of the **Arch-OpenRC** and **Manjaro-OpenRC** projects. Artix Linux offers a _lightweight_, _rolling-release_ operating system featuring the `OpenRC` init software. (An alternative spin features the `runit` init software.) Three editions of Artix are available, a minimal `Base` system, an edition featuring the `i3` window manager and an edition which runs the `LXQt` desktop.  
[主页：https://artixlinux.org/](https://artixlinux.org/)

## 安装详解:

> It is possible to use runit iso to install OpenRC-based system, and vice-versa.  
> 使用不同版本iso可以相互安装没有影响。

### 准备磁盘

- 使用fdisk进行硬盘分区 (这里使用/dev/sda)

```
fdisk /dev/sda
```

- 格式化分区（使用mkfs）

```
mkfs.ext4 -L ROOT /dev/sda1        <- root partition
 mkfs.ntfs -L HOME /dev/sda2        <- home partition, optional
 mkfs.ext4 -L BOOT /dev/sda3        <- boot partition, optional
 mkswap -L SWAP /dev/sda4           <- swap partition
```

> - The -L switch assigns labels to the partitions, which helps referring to them later through /dev/disk/by-label without having to remember their numbers.
> - 使用 `mkfs.ntfs` 时需要 `ntfs-3g`

- 挂载分区

```
mount /dev/sda1 /mnt
 mount /dev/sda2 /mnt/home  (if created)
 mount /dev/sda3 /mnt/boot  (if created)
 swapon /dev/sda4
```

## 安装base系统

- 修改本地镜像

编辑 `/etc/pacman.d/mirrorlist` ，加入本地镜像，目前腾讯云和清华大学的镜像可用。

```
Server = https://mirrors.cloud.tencent.com/artixlinux/$repo/os/$arch    # 腾讯云
Server = https://mirrors.tuna.tsinghua.edu.cn/artixlinux/$repo/os/$arch    #清华大学
```

编辑 `/etc/pacman.d/mirrorlist-arch` ，注释掉 `Worldwide` ，选择 `China` 取消注释。

> 安装的时候一定要先修改镜像地址，不然安装速度让人发狂。

- 更新软件仓库

```
pacman -Syy
```

- 安装系统

使用 `basestrap` 安装 `base`, init系统 (目前 `openrc` 或 `runit` 可用)， `base-devel` 选装。

```
basestrap /mnt base base-devel openrc
```

- 使用 `fstabgen` 生成 `/etc/fstab`

```
fstabgen -L /mnt >>/mnt/etc/fstab
```

> \-U for UUIDs  
> \-L for partition labels:

- chroot 进入新安装的Artix系统

```
artools-chroot /mnt
```

## 配置base系统

- 安装启动项： `grub` 和 `os-prober `

```
pacman -S grub os-prober
 grub-install --recheck /dev/sda
 grub-mkconfig -o /boot/grub/grub.cfg
```

- 创建用户和密码

```
useradd  user -g wheel -m
 passwd user
```

- 设置root密码

```
passwd
```

- 生成 `locales`:

```
nano /etc/locale.gen  <- uncomment your locale
 locale-gen
```

> 配置系统全局locale：编辑 ` /etc/locale.conf` (sourced by /etc/profile) 或  
> `/etc/bash/bashrc.d/artix.bashrc` 或 `/etc/bash/bashrc.d/local.bashrc` ；  
> 配置用户级locale： `~/.bashrc`

```
export LANG="en_US.UTF-8"
 export LC_COLLATE="C"
```

- 安装 `networkmanager`

```
pacman -S networkmanager networkmanager-openrc network-manager-applet
 rc-update add NetworkManager default
```

## 安装完成

```
exit   <- exit chroot environment
 umount -R /mnt
 reboot
```

