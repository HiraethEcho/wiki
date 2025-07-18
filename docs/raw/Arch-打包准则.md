---
title: "Arch 打包准则 -  Arch Linux 中文维基"
source: "https://wiki.archlinuxcn.org/wiki/Arch_%E6%89%93%E5%8C%85%E5%87%86%E5%88%99#%E6%89%93%E5%8C%85%E8%A7%84%E5%88%99"
date: 2025-06-08
description:
dg-publish: true
---
# Arch 打包准则**


在为 [Arch Linux](https://wiki.archlinuxcn.org/wiki/Arch_Linux "Arch Linux") 构建软件包时，您应该遵循以下的 **软件包指导原则** ，尤其是当打算贡献新软件包至 Arch Linux 时。同时需要阅读 [PKGBUILD(5)](https://man.archlinux.org/man/PKGBUILD.5) 和 [makepkg(8)](https://man.archlinux.org/man/makepkg.8) [man 手册](https://wiki.archlinuxcn.org/wiki/Man_%E6%89%8B%E5%86%8C "Man 手册") 。

本页中列出的重要项将不再于其它软件包指导页中指出，这些指导页将作为以下准则的附加内容。

提交到 [Arch 用户软件仓库 (AUR)](https://wiki.archlinuxcn.org/wiki/Arch_%E7%94%A8%E6%88%B7%E8%BD%AF%E4%BB%B6%E4%BB%93%E5%BA%93_\(AUR\) "Arch 用户软件仓库 (AUR)") 的软件包需要额外遵守 [AUR 提交准则](https://wiki.archlinuxcn.org/wiki/AUR_%E6%8F%90%E4%BA%A4%E5%87%86%E5%88%99 "AUR 提交准则") 。

可在 `/usr/share/pacman/` [目录](https://gitlab.archlinux.org/pacman/pacman/-/tree/master/proto) 下的 *.proto* 文件中查看更多 `PKGBUILD` 示例。

## 打包规则

- **永远别** 将软件包安装至 `/usr/local/`
- **除非非用不可，否则绝对不要在 `PKGBUILD` 中自定义和使用新的变量** ，以避免和 *makepkg* 本身的变量 **冲突** 。
- 在非用不可的情况下，需要 **给自定义变量名前加上下划线** (`_`)。例如：
	```
	_customvariable=
	```
- 任何情况下都要 **避免** 使用 `/usr/libexec/` ，应该使用 `/usr/lib/${pkgname}/` 。
- 包信息文件中的 `packager` 字段可以通过修改 `/etc/makepkg.conf` 文件由编译者进行 **自定义** 。使用 `~/.makepkg.conf` 也可以达到此目的。
- **不要使用 makepkg 子程序** （例如 `error` ， `msg` ， `msg2` ， `plain` 和 `warning` ），这些随时都可能会更改。请使用 `printf` 或 `echo` 来输出内容。
- 所有安装过程中所需输出的重要的信息，都要放到 **.install 文件中** 。比如说如果某软件包需要扩展的安装步骤才能正常运行，你可以将这些步骤的介绍包含在.install 文件中。
- **依赖包** 是最容易出现错误的地方。请花时间仔细核对，例如对动态可执行文件执行 `ldd` ，检查脚本需要的工具，查看软件文档等。 [namcap](https://wiki.archlinuxcn.org/wiki/Namcap "Namcap") 可以帮助你，它可以解析 PKGBUILD 和生成的打包文件，报告权限错误、缺少依赖、过多依赖等常见问题。
- 任何运行该软件包不需要，或者该软件包的通用功能不需要的 **可选依赖** 不要加入到 **depends** 中。这些信息应该加入 **optdepends** 数组，例如：

```
optdepends=('cups: printing support'
            'sane: scanners support'
            'libgphoto2: digital cameras support'
            'alsa-lib: sound support'
            'giflib: GIF images support'
            'libjpeg: JPEG images support'
            'libpng: PNG images support')
```

例子取自 [wine](https://archlinux.org/packages/?name=wine) <sup><small>包</small></sup> 软件包。这些信息在安装和升级时会自动打印，所以 **不要** 将这些信息加入.install 文件。

- 在填写 **软件包描述（description）** 时，请不要使用下定义的方式描述包名称。比如说，“Nedit is a text editor for X11” 应当简写为 "A text editor for X11"。顺便注意保持描述在 80 个字符以内.
- 尽量保持 `PKGBUILD` 文件中 **每行** 不超过100字符。
- 如果可能的话，从 `PKGBUILD` 文件中 **去掉空行** （没有设置变量值的行）(如 `provides` 、 `replaces` 等)
- 通常实践建议按照 [PKGBUILD](https://wiki.archlinuxcn.org/wiki/PKGBUILD "PKGBUILD") 一文中的示例 **安排 `PKGBUILD` 各变量顺序** 。当然这不是强制性的，这里唯一强制要求的是满足 **正确的 Bash 语法** 。
- 变量名可能包含空格时，请使用 **引号** ，例如 `"$pkgdir"` 和 `"$srcdir"` 。
- 请确保软件包的 **完整性** ，确保 [校验变量](https://wiki.archlinuxcn.org/wiki/PKGBUILD#%E5%AE%8C%E6%95%B4%E6%80%A7%E6%A3%80%E9%AA%8C "PKGBUILD") 包含正确的数值，可以通过 `updpkgsums` 工具进行更新。

## 软件包命名

- 软件包应当仅包含 **字母或数字** 以及 `@` ，`.`， `_` ， `+` ， `-` ，不能以 `-` 和 `.` 开头，所有的字母应当保持 **小写** 。
- 软件包的名称不应该包含上游的主版本号，例如如果上游软件包是 libfoo v2.3.4，不应该命名为 libfoo2。这样在上游发布新的大版本时，就可以保留软件包名和依赖可以保持不变。只有个别软件包是例外，例如图形库 GTK 和 Qt。这些软件升级后，应用程序需要花很长时间和经历才能移植完成，所以系统需要同时安装多个版本，软件包名应该包含大版本号，比如 gtk3、gtk4、qt5、qt6。如果出现大部分软件包都可以同步升级，只有个别软件没有被移植的情况，可以把老的软件包命名为 libfoo1，而新的软件包继续使用 libfoo。

## 软件包版本号

- 软件包 [版本号](https://wiki.archlinuxcn.org/wiki/PKGBUILD#pkgver "PKGBUILD") （即 `pkgver` ）应当 **和作者发行版号保持一致。**
- 如果需要的话，版本号可以包含字母（例如 `2.54BETA32` ）。
- **版本号里不能包含连字符** ，只能允许字母、数字、下划线、点号。如果上游版本号中出现了连字符，需要将其替换为下划线。

  

- 软件包 [发行号](https://wiki.archlinuxcn.org/wiki/PKGBUILD#pkgrel "PKGBUILD") （即 `pkgrel` ） **仅和 Arch 相关**. 这样用户就可以区分不同的编译版本。 **发布号从1开始** ，软件包被 **重新（打包）** 发布时 ， **发行号将增加1** 。
- 当新版本发布的时候，发行号（release）自动回到1。
- 软件包发布标记和软件包版本标记 **遵从同样的规则** 。

## 软件包依赖

- **不要依赖 [PKGBUILD#依赖关系](https://wiki.archlinuxcn.org/wiki/PKGBUILD#%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB "PKGBUILD") 中的 [传递依存关系](https://en.wikipedia.org/wiki/Transitive_dependency "wikipedia:Transitive dependency")** ，因为依赖更新可能会导致关系中断。
- 列出所有直接依赖库，可以通过 [find-libdeps(1)](https://man.archlinux.org/man/find-libdeps.1) （ [devtools](https://archlinux.org/packages/?name=devtools) <sup><small>包</small></sup> 的组件）来完成。

## 软件包关联

- 不要将 `$pkgname` 添加到 [PKGBUILD#provides](https://wiki.archlinuxcn.org/wiki/PKGBUILD#provides "PKGBUILD") ，该步骤由包本身隐式完成。
- 在 [PKGBUILD#provides](https://wiki.archlinuxcn.org/wiki/PKGBUILD#provides "PKGBUILD") 中列出所有外部共享库（如 `'libsomething.so'` ），可通过 [find-libprovides(1)](https://man.archlinux.org/man/find-libprovides.1) （ [devtools](https://archlinux.org/packages/?name=devtools) <sup><small>包</small></sup> 的组件）来完成。

## 软件源

- 尽可能使用 HTTPS 源：压缩包使用 `https://` ，git 源使用 `git+https://`
- 尽可能使用 PGP 签名对源进行验证（如果上游对提交进行了签名，但没有对压缩包签名，可能需要使用 git 标签而不是源码压缩包进行构建。）
- 使用 git 标签进行构建时，不要使用标签名，要使用 `git rev-parse` 获取到的对象哈希值

```
_tag=1234567890123456789012345678901234567890 # git rev-parse "v$pkgver"
source=(git+https://$url.git?signed#tag=$_tag)

pkgver() {
    cd "$pkgname"
    git describe
}
```

具体案例可参考 [gitea](https://archlinux.org/packages/?name=gitea) <sup><small>包</small></sup> 包。该操作的原因是，标签可以被强制推送改变指向到的提交，从而导致构建出的包产生变化。强制推送会改变标签的哈希值，从而使用标签的对象哈希值可以确保源的完整性。使用 `pkgver()` 函数可避免在没有更新 `_tag` 的情况下不小心修改掉 `pkgver` 。关于 VCS 源格式的更多信息请参考 [VCS 软件打包准则#VCS 源](https://wiki.archlinuxcn.org/wiki/VCS_%E8%BD%AF%E4%BB%B6%E6%89%93%E5%8C%85%E5%87%86%E5%88%99#VCS_%E6%BA%90 "VCS 软件打包准则") 。

- **Do not diminish the security or validity of a package** (e.g. by removing a checksum check or by removing PGP signature verification), because an upstream release is broken or suddenly lacks a certain feature (e.g. PGP signature missing for a new release)
- 源在 `srcdir` 下的名称不能相同（可能需要在下载时进行重命名，例如 `"${pkgname}-${pkgver}.tar.gz::https://${pkgname}.tld/download/${pkgver}.tar.gz"` ）
- 避免使用特定镜像源（如 sourceforge）进行下载，因为它们有可能会变得不可用

## 与上游协作

It is considered best-practice to work closely with upstream wherever possible. This entails reporting problems about building and testing a package.

- Report problems to upstream right away.
- Upstream patches wherever possible.
- Add comments with links to relevant (upstream) bug tracker tickets in the [PKGBUILD](https://wiki.archlinuxcn.org/wiki/PKGBUILD "PKGBUILD") (this is particularly important, as it ensures, that other packagers can understand changes and work with a package as well).

It is recommended to track upstream with tools such as [nvchecker](https://archlinux.org/packages/?name=nvchecker) <sup><small>包</small></sup>, [nvrs](https://aur.archlinux.org/packages/nvrs/) <sup><small>AUR</small></sup> or [urlwatch](https://archlinux.org/packages/?name=urlwatch) <sup><small>包</small></sup> to be informed about new stable releases.

## 目录

- **配置文件** 应该放置到 `/etc` 目录。如果有多个配置文件，可以使用 **子目录** ，以保持 `/etc` 简洁。例如，如果包名是 `*pkg*` ，那就应使用 `/etc/*pkg*` （或者其他合适的名称，比如apache使用的就是 `/etc/httpd/` ）。
- 软件包文件应该安装在下列 **常用目录** ：

| `/etc` | **系统关键** 配置文件 |
| --- | --- |
| `/usr/bin` | 二进制文件 |
| `/usr/lib` | 库 |
| `/usr/include` | 头文件 |
| `/usr/lib/*pkg*` | 模块，插件等 |
| `/usr/share/doc/*pkg*` | 应用程序文档 |
| `/usr/share/info` | GNU Info 系统文件 |
| `/usr/share/licenses/*pkg*` | Application licenses |
| `/usr/share/man` | 手册 |
| `/usr/share/*pkg*` | 程序数据 |
| `/var/lib/*pkg*` | 应用持久数据 |
| `/etc/*pkg*` | *pkg* 应用的配置文件 |
| `/opt/*pkg*` | 大的独立程序，例如 Java |

- 软件包不应该在下面目录添加任何文件:
	- `/bin`
	- `/sbin`
	- `/dev`
	- `/home`
	- `/srv`
	- `/media`
	- `/mnt`
	- `/proc`
	- `/root`
	- `/selinux`
	- `/sys`
	- `/tmp`
	- `/var/tmp`
	- `/run`

## Makepkg 的任务

当您使用 makepkg 构建软件包时，makepkg 会自动执行如下功能:

- 检查软件包的 **依赖** 和 **构建依赖** 是否已安装
- 从服务器中 **下载源文件**
- **校验** 源文件的完整性
- **解压** 源文件
- 打上必要的 **补丁（patch）**
- **构建** 软件并安装到 fake root
- 从可执行文件中 **去掉** 符号标记
- 从库文件中 **去掉** 调试标记
- **压缩** 手册页和/或 info 页
- 生成 **包信息文件** （包含软件包的基本信息）
- **压缩** fake root 成为软件包文件（\*.pkg.tar.xz）
- 生成的软件包文件 **保存** 在指定的目录中（默认在 当前目录 ）

## 架构

如果该软件包是针对特定架构编译的，那么 `arch` 数组应该包含 `'x86_64'` ，否则使用 `'any'` 生成架构无关的包。

## 授权协议

请参考 [PKGBUILD#license](https://wiki.archlinuxcn.org/wiki/PKGBUILD#license "PKGBUILD")

## 可复现构建

Arch 正努力使所有软件包具有 [可复现性](https://wiki.archlinuxcn.org/wzh/index.php?title=Reproducible_builds&action=edit&redlink=1 "Reproducible builds（页面不存在）") 。打包人员可通过 [devtools](https://archlinux.org/packages/?name=devtools) <sup><small>包</small></sup> 的 `makerepropkg` 或 [archlinux-repro](https://archlinux.org/packages/?name=archlinux-repro) <sup><small>包</small></sup> 的 `repro` 来检查包是否有可复现性：

```
$ makerepropkg $pkgname-1-1-any.pkg.tar.zst
```

或

```
$ repro -f $pkgname-1-1-any.pkg.tar.zst
```

如果在构建时需要使用时间戳，可以使用 `SOURCE_DATE_EPOCH` 环境变量，具体格式可参考 [上游文档](https://reproducible-builds.org/docs/source-date-epoch/) 。