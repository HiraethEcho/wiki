---
title: Basic intall of archlinux 
tags:
  - arch
---
# Basic installation of arch linux

## 启动盘

下载archlinux的iso文件。找一个U盘，用 `ventoy`安装，然后把iso放进去。

可以先在win下用 `Win`+`k`打开磁盘管理，然后手动分区，用win的GUI代替之后的命令行分区

插上U盘，重起电脑，在开机时按 `F11` `F12` `F2` 之类能进入选择启动位置的界面，选择从U盘启动。之后应该会启动U盘里的`ventoy`，选择arch的iso。这样就进入arch live 的系统，这个系统是用来安装arch的。

## 最小安装

### network and time

set up network:

```sh
iwctl
[iwd] station wlan0 connect AMSS
```

time

```sh
timedatectl set-ntp true
timedatectl status
```

change source for pacman. add followign in `/etc/pacman.d/mirrorlist`

```text
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
```

### 分区

```sh
fdisk -l
```

看清楚各个分区，以下`nvme0n1p5`等分区根据自己的分区情况调整。

Using `btrfs` file system, which is convenient to back up and restore.

```sh
mkfs.btrfs  -L btrfs-arch /dev/nvme0n1p5 # -L means label, can be omitted
mkswap /dev/nvme0n1p6    #  swap
```

mount and create subvolume

```sh
mount -o compress=lzo /dev/nvme0n1p3 /mnt

btrfs subvolume create /mnt/@
btrfs subvolume create /mnt/@home
btrfs subvolume create /mnt/@log
btrfs subvolume create /mnt/@tmp
btrfs subvolume create /mnt/@cache
btrfs subvolume create /mnt/@snapshots
```

check

```sh
btrfs subvolume list /mnt
```

should looks like

```sh
ID 256 gen 405 parent 5 top level 5 path @
ID 257 gen 409 parent 5 top level 5 path @home
ID 258 gen 409 parent 5 top level 5 path @log
ID 259 gen 404 parent 5 top level 5 path @tmp
ID 261 gen 310 parent 5 top level 5 path @cache
ID 262 gen 288 parent 5 top level 5 path @snapshots
```

`snapshots` 是之后用来存储备份的，`tmp` `cache` `log` 是不用备份的部分。尤其`logs`可以在崩溃回滚后查看日志
Then unmount

```sh
umount /mnt
```

### Mount subvolume 挂载分区

```sh
mount -o noatime,nodiratime,compress=lzo,subvol=@ /dev/nvme0n1p3 /mnt
mkdir -p /mnt/{boot/efi,home,var/{log,cache},tmp,.snapshots}
mount -o noatime,nodiratime,compress=lzo,subvol=@home /dev/nvme0n1p3 /mnt/home
mount -o noatime,nodiratime,compress=lzo,subvol=@log /dev/nvme0n1p3 /mnt/var/log
mount -o noatime,nodiratime,compress=lzo,subvol=@tmp /dev/nvme0n1p3 /mnt/tmp
mount -o noatime,nodiratime,compress=lzo,subvol=@cache /dev/nvme0n1p3 /mnt/var/cache
mount -o noatime,nodiratime,compress=lzo,subvol=@snapshots /dev/nvme0n1p3 /mnt/.snapshots
```

disable cow 关闭写时复制

```
chattr +C /mnt/tmp
chattr +C /mnt/var/cache
```

enable swap `swapon /dev/sda2`

mount efi

```sh
mount /dev/nvme0n1p1 /mnt/boot/efi
```

Note that if 在双系统安装，win初始的efi分区大小可能不够 `\boot` ，所以可以只挂载 `\boot\efi`。在win下可以用 `diskgenuis` 查看efi分区。

### install and genfstab 安装系统并生成分区表

```sh
pacstrap -K -M /mnt base base-devel vim base base-devel linux linux-firmware btrfs-progs networkmanager neovim git sudo grub os-prober efibootmgr amd-ucode btrfs-assistant # 安装linux和一些常用软件
genfstab -U /mnt >> /mnt/etc/fstab
```

根据cpu安装 `amd-ucode` 或 `intel-ucode`

- -G Avoid copying the host’s pacman keyring to the target.
- -i Prompt for package confirmation when needed (run interactively).
- -K Initialize an empty pacman keyring in the target (implies -G).
- -M Avoid copying the host’s mirrorlist to the target.

现在就在电脑的硬盘分区里安装里archlinux

### chroot and set up

进入到安装的系统中并做基本配置
chroot by `arch-chroot /mnt` 这样就进入到了刚刚安装的电脑里的archlinux

add archlinuxcn 为包管理器 `pacman`添加软件源，可以跳过。
在 `/etc/pacman.conf`中添加

```/etc/pacman.conf
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

set locale etc

```sh
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime #替换Region/City为你所在区域
hwclock --systohc # 同步硬件时钟
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
systemctl enable NetworkManager.service # 启动网络服务
hostnamectl set-hostname YOUR-HOSTNAME
```

`vim /etc/mkinitcpio.conf`
添加 btrfs 到 MODULES=(...)行
找到 HOOKS=(...)行，更换fsck为btrfs
最终你看到的/etc/mkinitcpio.conf文件格式为

```/etc/mkinitcpio.conf
MODULES=(btrfs)
HOOKS=(base udev autodetect modconf block filesystems keyboard btrfs)
```

然后重新生成 initramfs，`mkinitcpio -P`

#### add user 添加用户

```sh
passwd root
useradd -m -G wheel  USERNAME
passwd USERNAME
```

权限：之后运行 `visudo`，如果环境变量没有指定默认编辑器，会提示选择，选择一个之后会进入文件编辑界面，找到 `# %wheel ALL=(ALL:ALL)` 一行，删除最前面的井号注释，这样所有在 wheel 用户组的用户都可以使用 sudo 命令了，或者要是不想每次运行 sudo 都要输入密码，可以取消注释 `%wheel ALL=(ALL:ALL) NOPASSWD: ALL` 这一行，但这样可能会降低系统的安全性。

#### install grub

这一步让电脑启动时找到硬盘里的archlinux

先做一些配置：去掉 quiet 参数，调整 loglevel 值为 5 ，加入 nowatchdog 参数

```
sed -i "s|GRUB_CMDLINE_LINUX_DEFAULT.*|GRUB_CMDLINE_LINUX_DEFAULT=\"loglevel=5 nowatchdog\"|" /etc/default/grub
```

安装grub

```sh
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=arch --recheck
```

生成grub文件

```
grub-mkconfig -o /boot/grub/grub.cfg
```

### quit and reboot

```
exit #退出chroot
umount /mnt/boot/efi
umount /mnt/home
umount /mnt
reboot
```
## 基本软件
To be continue
