---
title: "Pake 如何将自己的页面打包成应用"
source: "https://www.master-jsx.top/archives/da-bao"
date: 2025-06-07
description: "使用 Pake 将自己的页面打包成应用，MacOs专属！"
dg-publish: true
---

# 如何将自己的页面打包成应用

```bash
# 下载pake组件来打包
npm install -g pake-cli

# 下载Rust开发环境{mac}
brew install rust

# 下载Rust开发环境{windows/linux}
https://forge.rust-lang.org/infra/other-installation-methods.html

# 打包网站
pake https://www.baidu.com --name Baidu
```

### url

`url` 是您需要打包的网页链接 🔗 或本地 HTML 文件的路径，此参数为必填。

### options

您可以通过传递以下选项来定制打包过程：

#### name

指定应用程序的名称。如果在输入时未指定，系统会提示您输入。建议使用英文名称。

```bash
--name <value>
```

#### icon

指定应用程序的图标，支持本地或远程文件。默认使用 Pake 的内置图标。您可以访问 [icon-icons](https://gitee.com/link?target=https%3A%2F%2Ficon-icons.com) 或 [macOSicons](https://gitee.com/link?target=https%3A%2F%2Fmacosicons.com%2F%23%2F) 下载自定义图标。

- macOS 要求使用 `.icns` 格式。
- Windows 要求使用 `.ico` 格式。
- Linux 要求使用 `.png` 格式。

```bash
--icon <path>
```

#### height

设置应用窗口的高度，默认为 `780px` 。

```bash
--height <number>
```

#### width

设置应用窗口的宽度，默认为 `1200px` 。

```bash
--width <number>
```

#### transparent

设置是否启用沉浸式头部，默认为 `false` （不启用）。当前只对 macOS 上有效。

```bash
--transparent
```

#### fullscreen

设置应用程序是否在启动时自动全屏，默认为 `false` 。使用以下命令可以设置应用程序启动时自动全屏。

```bash
--fullscreen
```

#### multi-arch

设置打包结果同时支持 Intel 和 M1 芯片，仅适用于 macOS，默认为 `false` 。

##### 准备工作

- 注意：启用此选项后，需要使用 rust 官网的 rustup 安装 rust，不支持通过 brew 安装。
- 对于 Intel 芯片用户，需要安装 arm64 跨平台包，以使安装包支持 M1 芯片。使用以下命令安装：

```csharp
rustup target add aarch64-apple-darwin
```

- 对于 M1 芯片用户，需要安装 x86 跨平台包，以使安装包支持 Intel 芯片。使用以下命令安装：

```csharp
rustup target add x86_64-apple-darwin
```

##### 使用方法

```bash
--multi-arch
```

#### targets

选择输出的包格式，支持 `deb` 、 `appimage` 或 `all` 。如果选择 `all` ，则会同时打包 `deb` 和 `appimage` 。此选项仅适用于 Linux，默认为 `all` 。

```bash
--targets <value>
```

#### user-agent

自定义浏览器的用户代理请求头，默认为空。

```bash
--user-agent <value>
```

设置是否显示菜单栏，默认不显示。在 macOS 上推荐启用此选项。

```bash
--show-menu
```

#### show-system-tray

设置是否显示通知栏托盘，默认不显示。

```bash
--show-system-tray
```

#### system-tray-icon

设置通知栏托盘图标，仅在启用通知栏托盘时有效。图标必须为 `.ico` 或 `.png` 格式，分辨率为 32x32 到 256x256 像素。

```bash
--system-tray-icon <path>
```

#### iter-copy-file

当 `url` 为本地文件路径时，如果启用此选项，则会递归地将 `url` 路径文件所在的文件夹及其所有子文件复

制到 Pake 的静态文件夹。默认不启用。

```bash
--iter-copy-file
```

#### inject

使用 `inject` 可以通过本地的绝对、相对路径的 `css` `js` 文件注入到你所指定 `url` 的页面中，从而为

其做定制化改造。举个例子：一段可以通用到任何网页的广告屏蔽脚本，或者是优化页面 `UI` 展的 `css` ，你

只需要书写一次可以将其通用到任何其他网页打包的 `app` 。

```bash
--inject ./tools/style.css --inject ./tools/hotkey.js
```

#### safe-domain

这个安全域名是除你当前配置的 `url` 之外可能会出现重定向或跳转到的其他域名，只有在已配置为安全的域名中，

才能够使用 `tauri` 暴露到浏览器的 `api` ，保证 `pake` 内置增强功能的正确运行。

PS: 安全域名不需要携带协议。

```bash
--safe-domain weread.qq.com,google.com
```

