---
title: 用btrfs做linux的快照备份
toc: true
tags:
  - arch
date:
---

# btrfs 的备份

## 手动备份

转移`home`

```
sudo rsync -avrh --progress /home/ /mnt/backup/
```

## timeshift and snapper

## timeshift 备份根目录

## snapper 备份子卷

snapper更加灵活。首先创建一个snapper的配置文件

```sh
sudo snapper -c <name of config> create-config <path-to-subvol>
```

例如对根目录的备份配置，`snapper`默认配置名是`root`

```sh
sudo snapper -c root create-config /
```

`snapper`默认会创建子卷`/.snapshots`，~~这不`arch`~~主要是和之前的子卷布局不协调，所以

```
sudo umount /.snapshots
sudo rm -r /.snapshots
sudo btrfs subvolume delete /.snapshots
sudo mkdir /.snapshots
sudo mount -o subvol=/ /dev/nvme0n1p1 /mnt
sudo btrfs subvolume create /mnt/@snapshots
```

### 从快照中恢复

复杂的手动方法，要从live USB开始，就是安装`arch`时的live系统

```sh
sudo mount /dev/nvme0n1p6 /mnt
cd /mnt
sudo mv /mnt/@ /mnt/@.broken
sudo btrfs subvolume snapshot /mnt/@snapshots/{number}/snapshot /mnt/@
sudo btrfs subvolume delete /mnt/@.broken
```

不进入live系统的方法，看上去很危险，最好回滚后立刻重启

```sh
sudo mount -o subvol=/ /dev/nvme0n1p1 /mnt
sudo btrfs subvolume snapshot /mnt/@ /mnt/@bad
sudo btrfs subvolume delete /mnt/@
sudo btrfs subvolume snapshot /mnt/@snapshots/要恢复的快照号/snapshot /mnt/@
```

可以用脚本完成回滚

```sh
#!/bin/sh
set -e
if [[ x"$1" == x ]]; then
  echo "No snapshot number given." 1>&2
  echo "Usage: rollback [snapshot to rollback]" 1>&2
  exit 1
fi
root_dev=`findmnt -n -o SOURCE / | sed 's/\[.*\]//g'`
root_subvol=`findmnt -n -o SOURCE / | sed 's/.*\[\(.*\)\].*/\1/'`
echo ">= Rollback to #$1 on device $root_dev"
# create snapshot before
sudo snapper create --read-only --type single -d "Before rollback to #$1" --userdata important=yes
sudo mount -o subvol=/ $root_dev /mnt
# check enviornment
if [[ x"$root_subvol" == x/@ ]]; then
  echo "Warning: Not run in a snapshot, a subvolume @old will be created. You should consider remove it after reboot." 1>&2
  if [[ -d /mnt/@old ]]; then
    echo "Found last @old, remove it."
    sudo btrfs subvolume delete /mnt/@old >/dev/null
  fi
  sudo mv /mnt/@ /mnt/@old
else
  sudo btrfs subvolume delete /mnt/@ >/dev/null
fi
sudo btrfs subvolume snapshot /mnt/@snapshots/$1/snapshot /mnt/@ >/dev/null
sudo umount /mnt
```

## Reference

1.  [ArchWiki-Btrfs](https://wiki.archlinux.org/title/Btrfs)
2.  [Working with Btrfs – General Concepts](https://fedoramagazine.org/working-with-btrfs-general-concepts/)
3.  [Working with Btrfs – Subvolumes](https://fedoramagazine.org/working-with-btrfs-subvolumes/)
4.  [Working with Btrfs – Snapshots](https://fedoramagazine.org/working-with-btrfs-snapshots/)
5.  [ArchWiki-Snapper](https://wiki.archlinux.org/title/snapper)
6.  [BTRFS snapshots and system rollbacks on Arch Linux](https://www.dwarmstrong.org/btrfs-snapshots-rollbacks/)
