---
title: "Kernel Panic: 我把 Arch 滚挂了"
date: 2025-10-13
---

- description: 每一位 Arch Linux 用户都喜欢做的事情： sudo pacman -Syu; 但是我自己犯了大错，在更新过程，终端卡死，键盘鼠标无响应，等了 20 几分钟也无响应，就长按电源强制关机了，再次开机后，我看到了 Kernel Panic
- source: [url](https://evex.one/posts/linux/kernel-panic-after-upgrade-arch-packages/)
- author: EXEC

This page looks best with JavaScript enabled

· ☕ 3 min read

## 美好的早晨

因为疫情，最近在家远程办公呢，像往常一样，我每天早上起床做的第一件事情就是解锁我的 `Linux` 系统，打开终端，执行：

```
1

sudo pacman -Syu
```

像往常一样，我等待着更新的过程，但是这次，我发现在下载并校验完待更新的包之后，在快要进入升级过程末尾的 `commit changes` 之前时，终端卡死，键盘鼠标无响应，连 `tty` 都进不去，等了 20 分钟都没响应，就长按电源强制关机了，等到我再次开机之后，看到了这样的画面：

## Kernel Panic

![kernel panic](https://evex.one/posts/linux/kernel_panic_caused_by_force_shutdown_on_upgrading/linux_panic_after_force_shutdown_in_upgrading.jpg)

可以看到： `error while loading shared libraries: /usr/lib/libcap.so.2: file too short` ，这应该是触发 `Kernel Panic` 的原因。

## 想办法抢救

很不幸，我没有把我 刻录着 `ArchISO` 的 `U` 盘带回家，所以我必须得想办法下载一个 `ArchISO` ，然后通过一种安装媒介来启动我的电脑。

于是搜了一下，找到了 [DriveDroid](https://www.drivedroid.io/) 。

很不幸，我的安卓系统是没有 `root` 权限的 `HavocOS` ，而且我的 `OS` 版本是安卓 10， `DriveDroid` 不支持这么高的版本。

![havocos](https://evex.one/posts/linux/kernel_panic_caused_by_force_shutdown_on_upgrading/havoc_android_os_try_to_boot_linux_but_cannot.jpg)

于是我找邻居借了 `U` 盘，备份了他 `U` 盘里原有的数据，他的电脑是 `win10` ，于是我在他的电脑上使用了 `rufus` 刻录了一个 `ArchISO` 到 `U` 盘。

## 紧急抢救

接下来我就把 U 盘插到我的电脑上，开机，进入 `BIOS` ，把启动顺序调整为 U 盘，然后重启，进入 `ArchISO` ，选择第一个选项，进入终端，然后 `mount` 了我的磁盘， `chroot` 进我的老家。  
我当时想着，要不我再 `pacman -Syu` 升级一下？看看是啥结果，于是：

![no ls](https://evex.one/posts/linux/kernel_panic_caused_by_force_shutdown_on_upgrading/libcap_file_too_short.jpg)

`sudo pacman -Syu` 执行失败是因为我还没连上网，但是？ **我发现我连 `ls` 都用不了?**

### 先连上网

于是我用手机开热点，尝试使用 `wifi-menu` 来链接我的热点，但等我输入完 `wifi` 密码之后， `wifi-menu` 的返回值是 2。

查了 `wifi-menu` 的 `man` 手册，里面说 返回值= `2` 表示： `The connection attempt failed`.  
`wifi-menu` 的 `man` 手册里指出 `NETCTL_DEBUG` 这个环境变量可以给出更多的 `debug` 信息： [wifi-menu(1) man pages](https://man.archlinux.org/man/core/netctl/wifi-menu.1.en)

后来，我看到 `ifconfig` 的输出是这样的：

![ifconfig](https://evex.one/posts/linux/kernel_panic_caused_by_force_shutdown_on_upgrading/ifconfig.jpg)

我竟然连 `ip` 地址都获取不到，于是检查一下 `dhcpcd` ，发现是没有启动的，于是启动它

![dhcpcd](https://evex.one/posts/linux/kernel_panic_caused_by_force_shutdown_on_upgrading/ps_dhcpcd.jpg)

于是换 `USB` 共享手机移动网络，却又发现：

![wifi_menu exit as 2](https://evex.one/posts/linux/kernel_panic_caused_by_force_shutdown_on_upgrading/wifi_menu_exit_as_2.jpg)

日志里说我的网卡已经 `up` 了，我尝试把 网卡 `down` 掉，再 `up` ，还是不行。

又换成 `USB` 共享手机网络，着相当于有线链接，还是获取不到 `ip` 。。。

然后我又换成 `wpa_supplicant`, 手动连手机热点，终于连上网了。

然后我重新安装了 `libcap` 包，把我损坏的 `lib` 文件都重新安装了。我又觉得不放心，肯定还有很多文件都损坏了，只是我还没发现，于是我写了个脚本去全盘搜索那些 `size=0` 的文件。

果然，我还有很多文件都损坏了：

![pacman -Qkk](https://evex.one/posts/linux/kernel_panic_caused_by_force_shutdown_on_upgrading/pacman_qkk.png)

![empty0](https://evex.one/posts/linux/kernel_panic_caused_by_force_shutdown_on_upgrading/empth_bash.png)  
![empty1](https://evex.one/posts/linux/kernel_panic_caused_by_force_shutdown_on_upgrading/empty_kernel_header.png)

在 `/usr/lib` 下面，我有 `10366` 个文件的 `size` 都是 `0`

后来照着最后一次升级时的 `pacman.log` 我把和损坏的文件有关的包都重装了一遍，但是我还是不放心，后来有空了就将整个系统重新安装了。

## 反思

这次 `Kernel Panic` 是因为我长按电源强制关机导致内存中有些数据没有及时 `sync` 到磁盘上，导致了重要文件损坏。  
于是我搜了一下，发现了 `linux` 有这个东西： [Linux Magic System Request Key Hacks — The Linux Kernel documentation](https://docs.kernel.org/admin-guide/sysrq.html)

### Magic SysRq key

`SysRq` 是系统底层的快捷键。遇到系统问题，可以能尝试这些快捷键，而不是按住电源开关强制关机

首先 `SysRq` 需要开启

```
1

echo "1" > /proc/sys/kernel/sysrq
```

如果你希望在挂载分区和启动引导前就开启的话, 请在内核启动参数上添加 `sysrq_always_enabled=1`

记住这个激活命令的通用口诀是 “Reboot Even If System Utterly Broken” (或者"REISUB")。

更详细的信息可以参考这里：

[SysRq - ArchWiki](https://wiki.archlinux.org/title/keyboard_shortcuts#Kernel_\(SysRq\))

### SysRq 快捷键

开启 `SysRq` 后，就可以使用这些快捷键了:

| key | 说明 |
| --- | --- |
| Alt+SysRq+R+ Unraw | 从 X 收回对键盘的控制 |
| Alt+SysRq+E+ Terminate | 向 所有进程发送 SIGTERM 信号，让它们正常终止 |
| Alt+SysRq+I+ Kill | 向所有进程发送 SIGKILL 信号，强制立即终止 |
| Alt+SysRq+S+ Sync | 将待写数据写入磁盘 |
| Alt+SysRq+U+ Unmount | 卸载所有硬盘然后重新按只读模式挂载 |
| Alt+SysRq+B+ Reboot | 重启 |

### 晒晒我的笔记本 A 面

![empty0](https://evex.one/posts/linux/kernel_panic_caused_by_force_shutdown_on_upgrading/full_logo_A_cover_xps.jpg)

## 参考

1. [Linux Magic System Request Key Hacks — The Linux Kernel documentation](https://docs.kernel.org/admin-guide/sysrq.html)
2. [SysRq - ArchWiki](https://wiki.archlinux.org/title/keyboard_shortcuts#Kernel_\(SysRq\))

---

---

normal