---
title: qemu虚拟机
toc: true
tags:
  - tools
date: 2025-04-21
dg-publish: false
moved: true
---

# qemu虚拟机

## Install

Install pakages

```arch
paru -S qemu-base # basic
paru -S libvirt # 虚拟机一些api的封装
virtual-manager # GUI
paru -S dnsmasq bridge-utils # net
```

Add user into groups, and start services  
编辑文件`/etc/libvirt/libvirtd.conf`

```conf
unix_sock_group = "libvirt"
unix_sock_rw_perms = "0770"
```

```
sudo usermod -a -G libvirt $(whoami)
```

```sh
sudo systemctl enable libvirtd.service
```

## net

```sh
sudo virsh net-list --all
sudo virsh net-start --all
sudo virsh net-autostart default
```


## Windows

[windows iot LTSC](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-11-iot-enterprise-ltsc) 一个更迷你的win镜像
