---
title: 用btrfs做linux的快照备份 
toc: true
tags:
  - arch
date: 
---
# btrfs 的备份
## timeshift and snapper

## timeshift 备份根目录

## snapper 备份子卷
snapper更加灵活

选择使用
```
btrfs subvolume create /mnt/@snapshots # 如果使用 snapper 作为回滚工具，可以现在创建这个子卷，安装完成后再创建也可以
```
