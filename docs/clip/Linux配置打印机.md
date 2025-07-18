---
title: "终于可以在 Linux 下愉快打印了：Linux 发行版配置打印机攻略 - 少数派"
source: "https://ios.sspai.com/post/90194"
date: 2025-05-13
description: "在这篇文章里，且看我抽丝剥茧，抓住 Linux 打印机配置问题的本质，带大家一步步完成配置，在 Linux 下畅享优质、便捷的打印体验。"
dg-publish: true
---
![](https://cdnfile.sspai.com//2024/7/15/article/48aa4cdd-2842-18a8-0e1a-40a36af77052.jpeg?imageMogr2/auto-orient/thumbnail/!750x720r/gravity/center/crop/750x720/format/webp/ignore-error/1)

终于可以在 Linux 下愉快打印了 ： Linux 发行版配置打印机攻略

[爱拼安小匠](https://ios.sspai.com/u/1194230) 2024年07月15日

> 在办公用的操作系统领域，Windows 常年傲视群雄。而近年来，政、企等单位操作系统国产化的进程，让「竞争对手」Linux 的市场份额有所提高 —— 越来越多的政府机构、国有企业都将既有的 Windows 电脑更换为国产电脑，配备银河麒麟、统信 UOS 等国产 Linux 发行版。

不过，对于运维人员来说，面对办公必备的装备——打印机，配置工作并不是那么好做：

- 原本在 Windows 下，你可以轻松配置打印机。只需插上打印机，下载驱动，运行安装程序，安装完成后就可以马上打印了。
- 相反，包括国产 Linux 在内的各类发行版，打印机无法即插即用。配置步骤非常繁琐，配套软件、技术支持常常遍地难寻，还要处理各种可能的 Bug，挑战性不小。

好在，这些困难都是可以一一化解的。

笔者在给自己的 Linux 主力机配置打印机时，在一次次的「踩坑」中，积累了不少有用的经验。在这篇文章里，且看我抽丝剥茧，抓住 Linux 打印机配置问题的本质，带领大家一步步完成配置，最终在 Linux 下畅享优质、便捷的打印体验。

## 背景知识：Linux 系统的打印机管理机制

熟悉 Windows 的读者都知道，在 Windows 下，打印机作为硬件设备直接由系统管理，是要安装驱动的。安装打印机驱动的过程，和显卡、声卡、网卡等基础硬件如出一辙。

然而，在 Linux 则不同。打印机采用基于软件的 [**CUPS**](https://www.cups.org/) **（Common Unix Printer Service，通用 Unix 打印服务）** 来管理。CUPS 是苹果公司推出的打印机管理套件，广泛应用于 macOS 与各大 Linux 发行版。

在 Linux 下，打印机不需要安装专门的硬件驱动 <sup href="">1</sup>

Linux下，部分以USB方式连接的硬件可直接访问，无须安装专门的硬件驱动。例如，Android玩家熟悉的ADB，在Windows下需要安装ADB和Fastboot驱动，但在Linux下就只是改个udev配置文件的事情。

，相反你只需要根据你的打印机型号，为CUPS 提供相应的「驱动文件」。这里的「驱动文件」，并不是直接从系统底层操纵设备的驱动，相反更多相当于软件配置，包括（但不限于）打印机的配置文件、过滤器 <sup href="">2<p>过滤器（filter），用于将待打印文档转换成特定打印机可以识别的图像格式。</p></sup>等。

**CUPS 理论上支持市面上绝大部分的打印机，** 适用面非常广，支持（但不限于）以下几种连接：

- **USB** 有线连接
- **IPP** （Internet Printer Protocol，Internet 打印机协议）：普及度最高的网络打印机协议
- **LPD/LPR** ：另一种打印机协议，稳定性高
- **SMB** ：支持访问Windows共享的打印机
- **蓝牙** 连接

管理方面， **Linux 发行版普遍使用 system-config-printer 这款工具来管理打印机** ，类似于 Windows 控制面板的「设备和打印机」。它是通用于各款发行版的打印机管理工具，由 RedHat 团队开发，界面简洁、体积轻巧，操作流程有章可循。

部分桌面环境，如 Gnome、KDE、深度桌面（DDE），也提供了自己的打印机管理工具。 **为方便起见，接下来将全程使用 system-config-printer 来演示。** 其余桌面环境自带工具的操作思路是相通的 <sup href="">3</sup>

UKUI、Xfce、MATE等桌面环境没有自己的打印管理器，因此也会借助system-config-printer来管理打印机。

<sup href="">4<p>KDE的打印机管理工具，在界面和操作上与system-config-printer几乎一模一样。</p></sup>。

## 准备工作：安装必要的软件包

想要顺畅地使用 CUPS 来管理打印机，首先要安装一系列软件包。

一般地，像 Ubuntu、Deepin、Zorin OS 这样的「新手向」发行版，安装时就已经配置好了CUPS 支持，开箱即用。而对于 Arch Linux、Gentoo 等「玩家向」的发行版，则需要自行配置。

### 安装、启用 CUPS

通常 CUPS 主要包含以下常用软件包：

| 软件包名 | 说明 |
| --- | --- |
| cups | CUPS 的本体。通常各大发行版的 CUPS 软件包是一站式的（即「meta package」），能够帮你安装 CUPS 的各个核心组件 |
| cups-browsed | **用于搜索网络上采用 IPP 的打印机** ，也可以与其他电脑上安装的CUPS联动。若你希望连接 IPP 的打印机，则需要安装这个包。 |
| bluez-cups | **用于连接蓝牙打印机** 。该包属于 Linux 蓝牙协议栈 [BlueZ](https://www.bluez.org/) 的模块，若你希望通过蓝牙打印，则需要安装这个包。 |
| cups-pdf | CUPS 提供的 PDF 打印机， **用于将文档打印成PDF格式** 。类似于 Microsoft Print To PDF 与 Foxit PDF Printer。 |

> **注：** 除 CUPS 外，其余的都是可选组件。

各大 Linux 发行版都收录了 CUPS，按照以下命令安装（其中可选组件根据你的需求安装）。常用的发行版安装命令如下：

```shell
# Arch Linux
sudo pacman -Sy cups cups-browsed bluez-cups cups-pdf

# Debian及衍生版，如：Ubuntu / Deepin / 银河麒麟 / UOS
sudo apt update
sudo apt install cups cups-browsed bluez-cups cups-pdf

# OpenSUSE
sudo zypper install cups cups-browsed bluez-cups cups-pdf

# Fedora
sudo dnf install cups cups-browsed bluez-cups cups-pdf
```

安装完成后，你需要启动 CUPS 服务：

```shell
# 启用CUPS基本服务
sudo systemctl enable cups      # 启用CUPS服务
sudo systemctl start cups       # 立即启动CUPS服务

# 若安装了cups-browsed，你也要单独启用它，因为它是一个独立的服务
sudo systemctl enable cups-browsed
sudo systemctl start cups-browsed
```

### 安装打印机管理工具：system-config-printer

system-config-printer 同样收录在发行版的软件仓库中，可以直接安装。部分发行版（如Manjaro、Linux Mint、银河麒麟）也有预装。

常用的发行版安装命令如下：

```shell
# Arch Linux
sudo pacman -Sy system-config-printer

# Debian及衍生版，如：Ubuntu / Deepin / 银河麒麟 / UOS
sudo apt update
sudo apt install system-config-printer

# OpenSUSE
sudo zypper install system-config-printer

# Fedora
sudo dnf install system-config-printer
```

### 安装主机解析工具 nss-mdns

**CUPS 使用** [**Avahi**](https://avahi.org/) **来搜索网络打印机。Avahi 是 Linux 上用于** 搜索网络设备的客户端（基于 mDNS/DNS-SD 协议），兼容苹果的 [Bonjour](https://developer.apple.com/bonjour/) 服务。

但是，在有的电脑上，光有Avahi还不够 <sup href="">5</sup>

比如笔者安装Arch Linux的ThinkPad X200。

——CUPS 能搜索到打印机，但是只能解析打印机的主机名 <sup href="">6<p>局域网打印机的主机名有固定的格式，例如笔者用的打印机Brother DCP-B7535DW，主机名形如“BRNBxxxxxxECxxF.local”。</p></sup>，却无法解析主机名对应的 IP 地址。 **主机名并不是域名** ，仅靠主机名，没有 IP 地址，你是连不上打印机的。

为了补齐这一短板，我们还需要安装 nss-mdns 软件包，它为 Avahi 提供解析网络打印机等网络设备 IP 地址的支持。由于该软件包只是 Avahi 的可选包，我们必须手动安装。

```shell
# Arch Linux
sudo pacman -Sy nss-mdns

# Debian及衍生版，如：Ubuntu / Deepin / 银河麒麟 / UOS
sudo apt update
sudo apt install libnss-mdns

# OpenSUSE
sudo zypper install nss-mdns

# Fedora
sudo dnf install nss-mdns
```

### 安装打印机驱动包——Foomatic-db

要想正式开始打印，还需要安装打印机驱动包。笔者在上一章提到，这里的「驱动」相当于软件配置，由 CUPS 调用。想要在 Linux 下安装打印机驱动，就离不开 **Foomatic-db** 。

Foomatic-db 是一套打印机数据库，收录了各大品牌主流型号打印机的配置文件（以 XML 格式编写）。这些配置文件通过配套的 Foomatic-db-engine 工具，转换为 PPD（PostScript Printer Description，PostScript 打印机描述）文件，CUPS 就是根据 PPD 文件来操作打印机的。

一般各大发行版都提供了 Foomatic-db 数据库，以及开箱即用的 PPD 文件合集，无须安装 Foomatic-db-engine 即可使用。

常用发行版的安装命令如下， **注意不同发行版要安装的软件包差异极大，请注意区分** ：

```shell
# Arch Linux
sudo pacman -Sy foomatic-db foomatic-db-ppds                 # 采用开源协议分发的驱动，支持型号较多
sudo pacman -S foomatic-db-nonfree foomatic-db-nonfree-ppds  # 非开源驱动，通常是厂商的专有驱动程序。支持型号较少

# Debian及衍生版，如：Ubuntu / Deepin / 银河麒麟 / UOS
sudo apt update
sudo apt install foomatic-db foomatic-db-compressed-ppds     # 打印机驱动
sudo apt install foomatic-filters                            # 打印机过滤器

# Fedora
sudo dnf install foomatic-db foomatic-db-ppds foomatic-db-filesystem
```

> **注意：** OpenSUSE 的软件仓库没有收录 Foomatic-db。希望使用 OpenSUSE 的读者在评论区分享自己配置打印机的经验。

## 连接USB有线打印机

CUPS 与 Foomatic 驱动包安装好之后，我们就可以着手连接打印机了。如果以 USB 有线方式连接打印机，那么配置相对会简单很多。

在接下来的操作之前，你需要开启打印机，用 USB 线缆将打印机与电脑连接起来。

### 检查设备连接情况

连接之后，可以使用 lsusb 工具来检测打印机是否正确连接。 <sup href="">7</sup>

lsusb工具在发行版中自带。若没有安装，则请安装usbutils软件包。

在终端中运行 lsusb，此时会列出所有已连接的 USB 设备。

若打印机已连接，则列表中会显示它的厂商名和型号。

### 启动 system-config-printer，开始添加打印机

**通常 system-config-printer 会出现在程序列表里，命名为「打印设置」「打印机配置」等，图标为一台打印机。** 运行后的界面如下图所示：

![](https://cdnfile.sspai.com/2024/07/04/c097a2568b73b27d094a402e59c59f7b.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

system-config-printer 的主界面

点击工具栏的「添加」按钮，启动打印机添加向导。此时，程序会自动扫描当前已连接的打印机，需要等待几秒钟，USB 打印机才会出现在列表中。

![](https://cdnfile.sspai.com/2024/07/08/21bca46bd7eaa4c647cdc11a77d97b7c.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

笔者手上没有 USB 打印机，无法展示连接打印机后的截屏。实践中，连接的 USB打印机大致会出现在「输入 URI」这一项的下面。

选中 USB 打印机，然后点击「转发」按钮 <sup href="">8</sup>

“转发”系“Forward（继续，下一步）”的误译。

。 **此时程序会加载驱动列表，注意这个过程耗时较长，需要你耐心等待。**

### 选择厂商与驱动程序

在接下来的界面中，找到你打印机的厂商：

![](https://cdnfile.sspai.com/2024/07/06/94ab63c113bd09aacc61c2682281dce6.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

选择打印机的厂商

紧接着，选择打印机型号，随后在右侧会列出该型号支持的驱动。我们优先选择标注有「推荐」的项目。

![](https://cdnfile.sspai.com/2024/07/06/9e00aca5d2ec4150f69899064c22a90d.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

选择打印机型号，以惠普打印机为例。

### 为打印机命名

向导的最后一步，程序会让你给打印机命名。其中：

| 项目 | 说明 |
| --- | --- |
| 打印机名称 | 是打印机的唯一标识符。  由英文、数字与分隔符「-」组成，不能带空格。 |
| 描述 | 则是在各个软件的「打印」对话框里显示的打印机名称。  建议填写打印机型号、用途等易于分辨的名字。 |

![](https://cdnfile.sspai.com/2024/07/06/f86e13cc2a9b2b99423af9553bb7820f.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

填写打印机的名称与描述。

设置完成后，单击「应用」，打印机就添加到了列表中。此时程序会询问你是否打印测试页，你可以打印一张测试页来检查打印机的连接情况。 <sup href="">9</sup>

后续若要测试，你随时可以双击列表中的打印机，点击“打印测试页”。

![](https://cdnfile.sspai.com/2024/07/06/779225c64a09a0cca027e4d996482634.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

打印机添加成功。

如果打印机连接正常，稍等片刻就会开始打印。

记得曾经在单位折腾打印机，失败多次后终于选择了正确的驱动，那一刻，听到打印机工作的声响，仿佛就是见证奇迹的时刻，欣喜之情不言而喻。

![](https://cdnfile.sspai.com/2024/07/06/620989f8d1f653f84f0d119e28daac75.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

打印出的测试页类似于这样。

## Wi-Fi 无线连接打印机

越来越多的打印机开始摆脱数据线和网线的束缚，支持 Wi-Fi 连接，无疑提升了打印的便捷性。CUPS 对网络打印功能提供了完备的支持，局域网中的 Wi-Fi 打印机自然不在话下，操作步骤与 USB 连接大致相同。

### 准备工作

在接下来的操作之前，你需要：

- 确保打印机和电脑连接到同一局域网里；
- 打印机以固定 IP 地址的方式连接，因为后续的步骤需要直接用到打印机的 IP 地址。

### 选择打印机连接方式

启动 system-config-printer，点击工具栏上的「添加」按钮，打开添加打印机向导。

展开界面左侧「 **网络打印机** 」，稍等片刻，连接在局域网上的打印机就会出现在列表中。

![](https://cdnfile.sspai.com/2024/07/08/269a3a117d9408cb99d5c59be32f7d7b.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

列表中的Brother DCP-B7535DW就是搜索到的网络打印机。其中，第一次出现是以DNS-SD或LPD/LPR方式连接，而第二次出现则是以IPP方式连接。

往往一台打印机支持多种连接方式，包括 IPP、DNS-SD <sup href="">10</sup>

由DNS-SD承载IPP协议，连接打印机。

、LPD/LPR。因此，正如上面的插图所示， **同一台打印机有可能会在列表出现两次** （一个是IPP，另一个是其他协议）。你可以根据自己的需要来选择。

就笔者的经验而言：

- IPP 支持的设备最多，与之关联的 [IPP Everywhere™](https://www.pwg.org/ipp/everywhere.html) 还可让打印机真正「免驱」使用（不再依赖厂商的驱动程序）。但是，部分打印机使用IPP时性能不佳，甚至无法连接。 <sup href="">11<p>例如Brother DCP-B7535DW，使用IPP时无线传输速度慢，有时也无法连接。</p></sup>
- LPD/LPR 兼容性最强，可以解决一些设备的兼容性问题，性能不弱。 <sup href="">12<p>同样是DCP-B7535DW，使用LPD/LPR时，操作体验无比顺畅。</p></sup>
- DNS-SD 是 IPP 的备用连接方式，体验与 IPP 类似。

### 连接方式一：IPP Everywhere™

较新的打印机支持 IPP Everywhere™ 技术，无须准备驱动，直接连接打印机即可。因此不妨优先选择。

**第一步：** 在system-config-printer 中，以 IPP 方式连接的打印机，在列表中显示的格式为「 `打印机型号(<打印机型号名>._ipp._tcp.local)` 」。选中该项目，右侧的「连接」列表中会显示「 **Driverless IPP（免驱动的 IPP）** 」项目。

**第二步：** 点击「转发」按钮。此时依然会搜索驱动，稍微耐心等待。

**第三步：** 在驱动列表中，选择「Generic」，点击「转发」。

![](https://cdnfile.sspai.com/2024/07/08/8a5a6d59a33b21469c5c0c68985ea05c.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

驱动程序的厂商列表。选择第一项「Generic」。

然后，再依次选择「 **IPP Everywhere** 」→「 **Genetic IPP Everywhere Printer \[en\]** 」。这就是 IPP Everywhere 的驱动程序。点击「转发」按钮确认。

![](https://cdnfile.sspai.com/2024/07/08/600a1b5f551766002b5792cae27d9690.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

驱动程序列表，如图所示来选择。

**第四步：** 随后会有个选项，询问你的打印机是否安装了双工器（英文版选项为「Duplexer Installed」）。这里的「双工器（duplexer）」正确翻译应当为「双面打印器」，这是部分打印机可拆卸的双面打印组件。 <sup href="https://www.hp.com/us-en/shop/tech-takes/what-is-duplex-printing">13</sup>

Wilson, M. (2019, August 21). What is Duplex Printing? \[Web Article\]. HP® Tech Takes. [https://www.hp.com/us-en/shop/tech-takes/what-is-duplex-printing.](https://www.hp.com/us-en/shop/tech-takes/what-is-duplex-printing.)

就我的经验来说，不支持或者内置了双面打印的机型都无须勾选该选项。

![](https://cdnfile.sspai.com/2024/07/08/41e3a1181c9dbe0430e9073c24a02c22.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

「已安装双工器（Duplexer Installed）」选项。

**第五步：** 最后，向导会要求你输入打印机的名称与描述，方法与上一张添加 USB 打印机的步骤相同。

**完成向导后，建议打印一张测试页，用以检测 IPP 打印机的连接情况。**

### 连接方式二：LPD/LPR 协议

有的打印机能使用 IPP，但会出现各种兼容性问题，例如连接失败、传输缓慢等。那么，不妨改用 LPD/LPR 方式来连接。

> **注意：** LPD/LPR 需要配合打印机驱动使用。

**第一步：** 在system-config-printer 中，以 IPP 以外方式连接的打印机，在列表中显示的格式为「 `打印机型号(<打印机序列号>.local)` 」。不同品牌打印机会有不同的序列号。

选中该项目，右侧的「连接」列表中会显示「LPD/LPR，队列 xxxxx」项目（其中，「xxxxx」视厂家的不同而不同 <sup href="">14</sup>

例如，兄弟打印机的队列名为“BINARY\_P1”。

，通常不必更改）。

**第二步：** 点击「转发」按钮。此时依然会搜索驱动，稍微耐心等待。

**第三步：** 接下来的步骤和上一章「USB 连接打印机」相同——选择驱动、输入打印机名称和描述。这里不再赘述。

**为了检测连接状况，笔者同样建议你打印一份测试页。**

### 连接方式三：DNS-SD

DNS-SD 是一种用于检测局域网设备的协议。有的时候，system-config-printer 的添加打印机向导会有 bug，无法为你的打印机显示单独的IPP项目（也就是「Driverless IPP」）。

此时，如果你仍想使用 IPP，就可以尝试以 DNS-SD 连接打印机。连接成功后，系统就能以 IPP 方式操作打印机。

具体的操作方法与上文「连接方式一」大致相同。唯一区别是：选择打印机时，要选择「 `打印机型号(<打印机序列号>.local)` 」，随后在右侧列表中选择「通过 DNS-SD 的 IPP 网络打印机」。如下图所示。

![](https://cdnfile.sspai.com/2024/07/08/67a4d87927890bd06db1f05800ce5060.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

通过DNS-SD方式连接打印机

## 无线打印机无法连接？——手动解析 IP 地址

通常情况下，配合 nss-mdns 软件包，Avahi 会自动在后台解析打印机的 IP 地址，因此只要根据上一章的步骤就可以使用无线打印机。但是，因不明原因，在笔者的 Arch Linux 上，Avahi 解析 IP 地址的能力「失灵」了，导致无论采取何种协议，都无法用 Wi-Fi 连接打印机。

此时，可以考虑手动解析打印机的 IP 地址，并在添加打印机时，手工将主机名替换为 IP 地址。解析的方式是使用 Avahi 自带工具进行 mDNS 查询。

**第一步：** 启动 system-config-printer，打开添加打印机向导。待你的打印机扫描出来后，观察列表，括号中显示为「 `xxxxxxx.local` 」的地址就是打印机的主机名。

![](https://cdnfile.sspai.com/2024/07/08/210ce4970276a10cf718083b4545c98c.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

红框中，括号内的内容就是打印机的主机名。

**第二步：** 打开终端，运行以下命令，解析打印机地址。

```shell
avahi-resolve-host-name <主机名>
```

运行结果类似于下图：

![](https://cdnfile.sspai.com/2024/07/08/dc26fea4a1768900812471d133210fe9.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

此时已经解析出了打印机地址。

**第三步：** 记下打印机的 IP 地址，随后在添加打印机向导中，将打印机地址或主机名进行如下替换：

- **IPP 方式连接时：将 IPP 路径中的主机名替换为 IP 地** 址，例如 `ipp://Brother%20DCP-B7535DW%20series._ipp._tcp.local/` 替换为 `ipp://192.168.200.1/` 。 <sup href="">15<p>那一长串的“Brother..._ipp._tcp.local”就是主机名。</p></sup>
- **LPD/LPR 方式连接时：** 直接将「主机」文本框的内容改为 IP 地址。
![](https://cdnfile.sspai.com/2024/07/08/2304715f22e1c9f55f97b062365f9377.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

若以 LPD/LPR 方式连接，直接将主机名替换为打印机的IP地址即可。

**第四步：** 后续按照上一章的教程，正常添加打印机即可，这里不再赘述。

## 找不到驱动程序？

尽管 Foomatic-db 收录的驱动程序包罗万象，但是在实践中，找不到驱动的情况仍然比比皆是。笔者最深刻的体验是：仅靠 Foomatic-db，是无法「征服」我历来接触过的打印机型号的（涵盖 Canon、Epson、Brother 等品牌），因为它们根本就没被收录在数据库中。

若无法找到驱动程序，你就需要另辟蹊径，通过以下渠道来为你的打印机寻找驱动。

### 品牌官网

一些打印机品牌提供了完备的跨平台支持，提供了 Linux 平台下的驱动。你可以在打印机官方网站的「技术支持」板块下载。

#### 实战举例：下载兄弟（Brother）HL-3190CDW 打印机的驱动

**第一步：** 检索并打开兄弟打印机的技术支持页面。

![](https://cdnfile.sspai.com/2024/07/05/3255efead776b6ad53fa0e3c1dcce0d8.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

在必应中搜索到的兄弟打印机技术支持链接

**第二步：** 在 [技术支持页面](https://support.brother.com/g/b/productsearch.aspx?c=cn&lang=zh) 中，搜索打印机型号「HL-3190CDW」，随后进入产品页面。

![](https://cdnfile.sspai.com/2024/07/05/61e1b7150c6c2b1ab1ef23caf4aaf9cb.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

兄弟产品的技术支持页面，即可直接输入型号检索，也可分门别类检索。

**第三步：** 在产品页面中，可见兄弟公司同时提供了三大平台的驱动。选择「Linux」，此时你可以视你的发行版，选择 RPM 或 DEB 格式的驱动包。

![](https://cdnfile.sspai.com/2024/07/05/ecc3ea8256a39ef53018e2604b7dee74.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

兄弟打印机的驱动下载页面

点击「确认」后，网站会要求你重新选择语言。选择「English」后，点击下方的「Linux printer driver (deb package)」（或「rpm package」），按提示操作即可下载。

![](https://cdnfile.sspai.com/2024/07/05/15d9eb886183475b49c2c6b0d0324f82.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

**第四步，** 安装驱动包。你需要直接使用发行版底层的驱动管理工具（ `rpm` 或 `dpkg` ）来安装。

```shell
# Debian及其衍生发行版（Ubuntu、Deepin、银河麒麟、UOS等）
sudo dpkg -i hl3190cdwpdrv-1.0.2-0.i386.deb

# Red Hat及其衍生发行版（Fedora、RHEL、OpenSUSE、CentOS、AlmaLinux等）
sudo rpm -i hl3190cdwpdrv-1.0.2-0.i386.rpm
```

> **提醒：** 兄弟这类打印机大厂普遍保守，要经过多道步骤才能让你下载到驱动程序。你需要保持耐心。

**其他品牌打印机查找驱动的思路是相同的，不必担心找不着方向。通常大部分驱动都可以从官方网站找到** <sup href="">16</sup>

极个别对Linux支持不友好的品牌除外，例如一些只能在Windows下使用的行业专用打印机。

**。**

#### 其他部分厂家提供的 Linux 驱动

![](https://cdnfile.sspai.com/2024/07/05/4c832e9fa84f6b98a282210cdd249fc2.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

佳能官方技术支持网站

![](https://cdnfile.sspai.com/2024/07/05/e761ed90b3568175f40219cf02a5400c.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

爱普生官方技术支持网站 ，针对国产 Linux 发行版提供了多个处理器架构的驱动。

![](https://cdnfile.sspai.com/2024/07/06/f0d2c4b56f17721a2c7857084edd7001.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

得力打印机驱动程序。得力网站上找驱动程序很费劲，还得 在网站上搜索一番 才能找到能。

**一个特例是，惠普的 Linux 驱动由第三方的** [**HPLIP**](https://developers.hp.com/hp-linux-imaging-and-printing/) **项目提供支持** 。通常各大 Linux 发行版都提供了 [`hplip` 软件包](https://pkgs.org/search/?q=hplip) ，开箱即用。

![](https://cdnfile.sspai.com/2024/07/06/2ad0e7302c26a4d7b52d941e6d80bbf1.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

[HPLIP官方网站](https://developers.hp.com/hp-linux-imaging-and-printing/)

### Arch Linux用户：可以从软件源安装

如果你使用 Arch Linux 及其衍生版（Manjaro、SteamOS 等），查找驱动要更为方便。AUR 和 Arch Linux CN 软件源是个大宝库，往往Foomatic没有收录的驱动程序常常可以从这两个渠道找到。

其中：

- AUR 收录的品牌和型号最全，打印机驱动常常可以具体到某种型号。
- Arch Linux CN 收录的驱动程序很少。

> 注意：在继续之前，请确保你安装了 [AUR助手](https://wiki.archlinuxcn.org/wiki/AUR_%E5%8A%A9%E6%89%8B) ，例如 [Yay](https://wiki.archlinuxcn.org/wiki/Yay) 。下文将使用 Yay 来演示。

**第一步，** 为了确保你的目标品牌驱动程序有收录，你需要先用 `yay -Ss` 来检索打印机 **品牌名称** 。该命令同时也会检索 Arch Linux CN 源的内容。

例如，可分别使用以下四个命令，检索兄弟（Brother）、佳能（Canon）、爱普生（Epson）、三星（Samsung）这四个品牌的驱动程序：

```shell
yay -Ss brother
yay -Ss canon
yay -Ss epson
yay -Ss samsung
```

随后 Yay 会自动检索 AUR 软件源。仅仅是兄弟打印机，就可以搜索到近百个驱动，以下是搜索结果的节选：

```shell
...
aur/brother-hl1210w 3.0.1-2 (+10 0.07) 
    Brother HL-1210W CUPS driver. After installing this, install printer from CUPS web interface
aur/brother-hll2320d 3.2.0_1-1 (+7 0.00) 
    Brother HL-L2320D CUPS driver
aur/brother-lpr-drivers-common 1.0.0-1 (+54 0.00) 
    This package provides common files for Brother LPR drivers packages.
aur/brother-dcp7055 2.0.4-2 (+14 0.00) 
    LPR and CUPS driver for the Brother DCP7055
...
aur/brother-hl1118 3.0.2-1 (+12 0.00) 
    LPR and CUPS driver for the Brother HL-1110, HL-1110R, HL-1111, HL-1112, HL-1112R, HL-1118
aur/brother-hll2300d 3.2.0_1-1 (+11 0.02) 
    Brother HL-L2300D CUPS driver
aur/brother-hl2140 2.0.2_1-4 (+15 0.00) 
    LPR and CUPS driver for the Brother HL2140
aur/brother-brgenml1 3.1.0_1-3 (+32 0.00) 
    LPR and CUPS driver for various Brother DCP, HL and MFC models.
aur/brother-dcp7030 2.0.2-4 (+17 0.00) 
    Brother cupd and lpd driver for DCP-7030
aur/brother-hll2350dw 4.0.0-2 (+22 0.21) 
    Brother HL-L2350DW CUPS driver
...
aur/brother-hl2030 2.0.1-6 (+35 0.00) 
    Brother HL-2030 CUPS driver
aur/brother-hl3170cdw 1.1.4-1 (+10 0.00) 
    LPR and CUPS driver for the Brother HL3170CDW
archlinuxcn/brother-mfcj450dw 3.0.0-2 (1.5 MiB 4.5 MiB) 
    LPR and CUPS driver for the Brother MFC-J450DW
archlinuxcn/brother-hl3170cdw 1.1.4-1 (428.3 KiB 3.3 MiB) 
    LPR and CUPS driver for the Brother HL3170CDW
```

**第二步，** 检索具体的打印机型号。注意在检索型号时，要把型号拆开来写，例如兄弟 HL-2240 打印机，检索型号时的关键字为 `hl 2240` 。如下所示：

```shell
➜  ~ yay -Ss hl 2240
aur/brother-hl2240-lpr-bin 2.1.0-1 (+0 0.00) 
    LPR driver for Brother HL-2240 printer
aur/brother-hl2240-cups-bin 2.0.4-2 (+0 0.00) 
    CUPS wrapper for Brother HL-2240 printer
aur/brother-hl2240d 2.0.4_2-1 (+4 0.07) (已安装)
    LPR and CUPS driver for the Brother HL-2240D printer
```

> **注意：** 有的打印机驱动同时提供了独立的 LPR、CUPS 版本，应当优选 CUPS版，或二者兼备的版本。

**一个特殊的例子是三星打印机驱动** ，部分驱动程序同时适用于同系列的多款型号，所以输入具体型号反而检索不出来。此时你仍然需要直接检索厂商名（而非具体型号），翻翻列表，才能找到。

```shell
aur/samsung-ml191x-series 1.00.21-1 (+0 0.00) 
    CUPS printer driver for Samsung ML-191x Series
aur/samsung-ml1860series 1.00.21-1 (+1 0.00) 
    CUPS printer driver for Samsung ML-1860 Series
aur/samsung-m262x-m282x 1.00.36_00.91-2 (+1 0.00) 
    CUPS driver for Samsung Xpress M262x and M282x printers
```

**第三步，** 安装驱动程序。例如，安装兄弟 HL-2240D 的驱动程序：

```shell
yay -S brother-hl2240d
```

随后按提示操作即可。 <sup href="">17</sup>

其中有一个步骤是问你是否“显示差异”，直接回车将它pass掉。

### 在安装驱动后……

确认驱动安装完成后，你就可以在 system-config-printer 的驱动列表中找到它了。按照前文配置打印机的教程正常配置即可。

![](https://cdnfile.sspai.com/2024/07/06/f06cd166792050d41897d66eb47c86de.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

刚刚从 AUR 安装的 HL-2240D 驱动程序出现在了列表中

## 小技巧：OEM / 贴牌打印机找驱动

部分品牌的打印机，例如联想、柯尼卡美能达（Konica Minorta），它们的一些型号无论怎么找都未必能找到驱动。实际上，它们或许是「贴牌打印机」—— 表面上是 A 品牌，实际上不过是 B 品牌的打印机以 A 品牌的名义发售（即 OEM 产品）。

**往往，一台贴牌打印机，与它们对应的原厂型号机器在外观上极其相似。** 你可以通过对比外观，来找到贴牌机「表皮」下的真实型号，从而找到真正适用于该打印机的驱动。

下面就以笔者的经历，来告诉大家如何为贴牌打印机找驱动。

笔者曾经接触过一台打印机——柯尼卡美能达的 Bizhub 12p，要连接到自己的 Arch Linux 笔记本上。然而该驱动不仅没有被收录到 Foomatic-db 与 AUR 中，官方网站甚至根本不提供 Linux 平台下的驱动。

![](https://cdnfile.sspai.com/2024/07/06/f5cd3762a7708a71b0fb23ec45fff4b4.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

这台就是 Bizhub 12p

正当我一筹莫展的时候，我想起另一位朋友也安装了银河麒麟，配备的打印机是联想 LD 系列的一款激光打印机，外观和 Bizhub 12p 一样方方正正。于是我借他的电脑，查看这台打印机的驱动，竟然用的是兄弟打印机 HL 系列的一个型号。去兄弟的官网查询该型号，果然这台联想 LD 打印机与兄弟该型号的区别，就只是颜色不同、商标不同而已。

受此启发，我看了看 Bizhub 12p 的外观，想起兄弟的打印机也具有如此高辨识度的方正造型，我就判断出 Bizhub 12p 很可能是兄弟某款打印机的贴牌机。

果不其然，检索 [兄弟打印机的产品页面](https://www.brother.cn/printer/prt) ，我发现了意外惊喜：

![](https://cdnfile.sspai.com/2024/07/06/de5a4699b7418043e776a917a5115b1a.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

![](https://cdnfile.sspai.com/2024/07/06/7c3f1a613c8a4bcacc77f43e869e68a3.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

我就在这里发现了惊喜

![](https://cdnfile.sspai.com/2024/07/06/9fb277addd04b139e86ddc2a5f455cca.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1/format/webp)

HL-2240打印机（图源： 兄弟中国官方网站 ）

且看HL-2240、HL-2260D 这两款打印机，外观与 Bizhub 12p 非常相近。 **尤其是指示灯、按键的布局，就是一个模子里刻出来的。**

抓住这条线索，我立即连接 Bizhub 12p， **在自己的 Arch Linux 系统上，通过 AUR 安装了 HL-2240 的驱动程序，配置打印机，结果顺利打印了测试页** ！

至此，就可以证明 Bizhub 12p 为 HL-2240 的贴牌机，在 Linux 下也通用后者的驱动 <sup href="">18</sup>

顺便说一下，Bizhub 12p也可以使用HL-2240D的驱动。后者是HL-2240支持双面打印的版本。

。对于银河麒麟，由于我做测试那会儿，还没有 HL-2240 的驱动，于是在麒麟客服的建议下，先用 HL-2260D 的驱动，也成功带动了 Bizhub 12p。

如果你一直找不到打印机的驱动，尤其是一些非一线品牌的打印机，那么首先就可以假设它是贴牌机，然后从一些主流品牌的打印机入手（尤其是像兄弟这样经常做 OEM 产品的品牌），寻找与该打印机外观相似的型号，往往你就能找到合适的驱动程序 <sup href="">19</sup>

通常一线品牌是有提供Linux支持的，如惠普、佳能、兄弟等。即使驱动程序版本较老，也能保证可用性、稳定性。

。

## 写在最后

和 Windows 与 macOS 不同，Linux 下配置打印机是个技术活。为了让部分型号的打印机能正常使用，的确需要「折腾」一番，对使用国产 Linux 的政企客户来说更是如此。

正是在这样的背景下，笔者写下这篇打印机安装攻略，为大家「抽丝剥茧」，将 Linux 配置打印机的步骤和要点写清、写透。相信无论是运维人员，还是更广大的 Linux 发行版用户，在阅读这篇教程后，能够轻松配置打印机，在 Linux 下享受畅快、自然的打印体验。

期待本文能给大家帮助和启发。

\> 关注 [少数派公众号](https://sspai.com/s/J71e) 、 [小红书](https://www.xiaohongshu.com/user/profile/63f5d65d000000001001d8d4) ，感受精彩数字生活 🍃

\> 实用、好用的 [正版软件](https://sspai.com/mall) ，少数派为你呈现 🚀

- 1 Linux下，部分以USB方式连接的硬件可直接访问，无须安装专门的硬件驱动。例如，Android玩家熟悉的ADB，在Windows下需要安装ADB和Fastboot驱动，但在Linux下就只是改个udev配置文件的事情。
- 2 过滤器（filter），用于将待打印文档转换成特定打印机可以识别的图像格式。
- 3 UKUI、Xfce、MATE等桌面环境没有自己的打印管理器，因此也会借助system-config-printer来管理打印机。
- 4 KDE的打印机管理工具，在界面和操作上与system-config-printer几乎一模一样。
- 5 比如笔者安装Arch Linux的ThinkPad X200。
- 6 局域网打印机的主机名有固定的格式，例如笔者用的打印机Brother DCP-B7535DW，主机名形如“BRNBxxxxxxECxxF.local”。
- 7 lsusb工具在发行版中自带。若没有安装，则请安装usbutils软件包。
- 8 “转发”系“Forward（继续，下一步）”的误译。
- 9 后续若要测试，你随时可以双击列表中的打印机，点击“打印测试页”。
- 10 由DNS-SD承载IPP协议，连接打印机。
- 11 例如Brother DCP-B7535DW，使用IPP时无线传输速度慢，有时也无法连接。
- 12 同样是DCP-B7535DW，使用LPD/LPR时，操作体验无比顺畅。
- 13 Wilson, M. (2019, August 21). What is Duplex Printing? \[Web Article\]. HP® Tech Takes. [https://www.hp.com/us-en/shop/tech-takes/what-is-duplex-printing.](https://www.hp.com/us-en/shop/tech-takes/what-is-duplex-printing.)
- 14 例如，兄弟打印机的队列名为“BINARY\_P1”。
- 15 那一长串的“Brother...\_ipp.\_tcp.local”就是主机名。
- 16 极个别对Linux支持不友好的品牌除外，例如一些只能在Windows下使用的行业专用打印机。
- 17 其中有一个步骤是问你是否“显示差异”，直接回车将它pass掉。
- 18 顺便说一下，Bizhub 12p也可以使用HL-2240D的驱动。后者是HL-2240支持双面打印的版本。
- 19 通常一线品牌是有提供Linux支持的，如惠普、佳能、兄弟等。即使驱动程序版本较老，也能保证可用性、稳定性。

本文责编：@ [Lotta](https://ios.sspai.com/u/1034026)

© 本文著作权归作者所有，并授权少数派独家使用，未经少数派许可，不得转载使用。

热门评论

1ee

2024 年 07 月 15 日

非常感谢，刚好需要连打印机

少数派60724123

2024 年 12 月 20 日

欧拉ukui可以安装system-config-printer，打印设置图标也出来了，点了没反应，还得通过localhost:631的cups服务来添加。

<sup>17</sup>

<sup>60</sup>