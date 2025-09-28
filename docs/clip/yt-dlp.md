---
title: "被 YouTube 斩尽杀绝！yt-dlp 几乎被判死刑…但它像个斗士一样，又杀回来了！"
date: 2025-09-28
---

- description: 摘要：yt-dlp 开发者最近宣布，未来用户必须安装 Deno 或其他 JavaScript 运行时，才能正常下载 YouTube 视频。随着 YouTube
- source: [url](https://www.appinn.com/yt-dlp-returned-from-youtube-crackdown/)
- author: 青小蛙

**摘要** ：yt-dlp 开发者最近宣布，未来用户必须安装 Deno 或其他 JavaScript 运行时，才能正常下载 YouTube 视频。随着 YouTube 再次升级加密机制，这款开源下载工具不得不依靠第三方工具对抗技术封锁，自由下载的门槛被前所未有地提高。原文：https://www.appinn.com/yt-dlp-returned-from-youtube-crackdown/

![被 YouTube 斩尽杀绝！yt-dlp 几乎被判死刑…但它像个斗士一样，又杀回来了！ 1](https://do-cdn.appinn.com/wp-content/uploads/2025/09/Copy-of-appinn-homework-2025-09-27T193403.965.jpg)

被 YouTube 斩尽杀绝！yt-dlp 几乎被判死刑…但它像个斗士一样，又杀回来了！ 1

## youtube-dl

2009 年 7 月，Ricardo Garcia 把一段不到 200 KB 的 Python 脚本扔进 GitHub，取名 youtube-dl。

终端里敲下一行

```js
youtube-dl https://www.youtube.com/watch?v=appinncom.balabala
```

就能把一个 1080p 视频拖回硬盘 —— 没有登录、没有广告、没有额外依赖。

那一年，YouTube 的加密只是百行左右的 JavaScript，youtube-dl 用几行正则就把它剥得精光。

那是它的黄金时代，也是我们的黄金时代。

![被 YouTube 斩尽杀绝！yt-dlp 几乎被判死刑…但它像个斗士一样，又杀回来了！ 2](https://www.appinn.com/wp-content/uploads/2025/09/Screen-20250927182300@2x.avif)

被 YouTube 斩尽杀绝！yt-dlp 几乎被判死刑…但它像个斗士一样，又杀回来了！ 2

## 军备竞赛 2010-2019

随后，YouTube 迅速崛起为全球最大的视频网站，吸引了大量商业用户和内容创作者。

与此同时，美国唱片协会（RIAA）及其代表的环球音乐、索尼音乐等巨头的版权投诉，像雪片般飞向 GitHub（youtube-dl 托管在 GitHub），每一次 DMCA（数字千年版权法）下架通知，都让 youtube-dl 在生死线上挣扎。

![被 YouTube 斩尽杀绝！yt-dlp 几乎被判死刑…但它像个斗士一样，又杀回来了！ 3](https://www.appinn.com/wp-content/uploads/2025/09/Screen-20250927182600@2x.avif)

被 YouTube 斩尽杀绝！yt-dlp 几乎被判死刑…但它像个斗士一样，又杀回来了！ 3

但真正的致命打击来自 Google，YouTube 的母公司。

YouTube 工程师将 40 行视频加密代码扩展到数千行，签名算法从静态字符串升级为动态令牌。

youtube-dl 的战争形态就此改变：更新频率从 “月更” 压缩到 “日更”，开发者开始轮班盯守 GitHub Issues。

虽然 youtube-dl 安装包始终保持着轻量级，核心只是一个不到 1MB 的纯 Python 脚本。

但为了应对日益复杂的加密，脚本内部不断增加解析、兼容 YouTube 签名逻辑的代码，只靠 Python 的简易解释能力，和复杂的正则表达式“硬拆” YouTube 的加密“锁”。

用户苦笑着调侃： “我们的瑞士军刀，正在变成军火库。”

## youtube-dl 成为历史

2020年10月23日，美国唱片协会（RIAA）向 GitHub 发出 DMCA（数字千年版权法）下架通知，理由是 youtube-dl 项目据称规避了 YouTube 及其它流媒体服务的技术保护措施，可以下载受版权保护的内容，这被认为违反了 DMCA 第 1201 条反规避条款。

GitHub 随即删除了包括主仓库在内的 18 个 youtube-dl 相关代码库，并警告社区用户严禁再行上传副本，否则会面临账号封禁。这一决定引发了全球开发者与关注开源社区的强烈不满，许多人认为DMCA被“滥用”，GitHub 过度顺从版权机构。

甚至 GitHub 的 CEO、电子前线基金会（EFF）、大量技术媒体都公开发声支持 youtube-dl，质疑版权机构的行为。

![被 YouTube 斩尽杀绝！yt-dlp 几乎被判死刑…但它像个斗士一样，又杀回来了！ 4](https://www.appinn.com/wp-content/uploads/2025/09/Screen-20250927183005@2x.avif)

被 YouTube 斩尽杀绝！yt-dlp 几乎被判死刑…但它像个斗士一样，又杀回来了！ 4

这次下架事件成为开源软件历史上的重大转折点，也让 youtube-dl 一度“成为历史”。

此后，社区大量 fork、备份代码，涌现出新分支和替代项目，象征着原版 youtube-dl 的“黄金时代”正式画上了句号。

## yt-dlp 的时代

就在 youtube-dl 被 DMCA 压力重压、GitHub 仓库彻底下架后的几天里，社区并没有选择沉默。

2020年10月底，一个新的分支项目诞生 yt-dlp，它不仅复制了 youtube-dl 的全部功能，还迅速集结起更多的自由开发者，这个新项目的目标很明确：

- 继承 youtube-dl 的全部功能
- 规避可能的法律风险

开发者们在README中写道：”这不是简单的fork，而是进化。”

![被 YouTube 斩尽杀绝！yt-dlp 几乎被判死刑…但它像个斗士一样，又杀回来了！ 5](https://www.appinn.com/wp-content/uploads/2025/09/Screen-20250927183041@2x.avif)

被 YouTube 斩尽杀绝！yt-dlp 几乎被判死刑…但它像个斗士一样，又杀回来了！ 5

终端里那条熟悉的命令依然有效：

```js
youtube-dl https://www.youtube.com/watch?v=appinncom.balabala
```

但这一次，用户们发现安装包的体积已经翻了几番，功能也远超原版。除了需要自行安装 FFmpeg 等依赖外，这个“进化版”还内置了一个简易的 JavaScript 解释器，用于破解 YouTube 不断升级的加密逻辑。

没人能预料到，这仅仅是一场更大规模技术对抗的开始。

## YouTube 的新盾牌：PO Token

2024年6月，YouTube悄然上线了一种被社区称为“PO Token”（Playback Offset Token）的新技术手段。这种令牌不再是传统的静态签名，而是每隔五分钟自动失效、实时计算、一次性使用的动态令牌。

这一技术封锁让 yt-dlp 原有的 JavaScript 解释器失去了作用。上午还能正常下载的视频链接，下午却纷纷返回 403 Forbidden。

开发者曾尝试利用浏览器泄露的 token 勉强继续下载，但这些 token 极易过期，短时间内便全部失效。还有人尝试使用 Selenium、Playwright 等自动化工具模拟完整浏览器流程，结果是 CPU 飙升，内存爆满，普通笔记本已经无法承受。社区甚至构建了云端解密、Token 池、行为指纹对抗等新方案。

补丁越打越厚，墙却越升越高。

PO Token 的出现，把“下载”这件小事从“技术小把戏”变成了“实时军备竞赛”。没有实时 JavaScript 运行时的 yt-dlp，只能眼睁睁看着那道 5 分钟刷新一次的城门，在自己面前一次次关紧。

## yt-dlp 像个斗士一样回来了！

4天前（2025年9月23日），yt-dlp 开发者 bashonly [发布公告](https://github.com/yt-dlp/yt-dlp/issues/14404) ，坦言 YouTube 下载即将迎来全新门槛：“很快，您需要安装 Deno 或其它受支持的 JavaScript 运行时，才能继续下载 YouTube 的视频。”

![被 YouTube 斩尽杀绝！yt-dlp 几乎被判死刑…但它像个斗士一样，又杀回来了！ 6](https://www.appinn.com/wp-content/uploads/2025/09/Screen-20250927183128@2x.avif)

被 YouTube 斩尽杀绝！yt-dlp 几乎被判死刑…但它像个斗士一样，又杀回来了！ 6

公告中，他解释 YouTube 的反爬机制已升级到前所未有的高度，动态签名（PO Token）的实时算法让原有的内置 JavaScript 解释器彻底失效。开发者们尝试了各种修复方法，但每一次补丁都像在漏水的船上临时加固，最终还是必须引入真正的 JavaScript 运行环境作为根本解决方案。

新的方案意味着，今后的 yt-dlp 要依靠外部的 JS 引擎，才能穿透 YouTube 的技术壁垒。

对用户来说，这不仅是一次简单的命令变更，更意味着下载流程和环境要求将远比以往复杂。

但只要 yt-dlp 还在不断更新、还有开发者奋战，下载自由就不会消失。

值得庆幸的是，除了 YouTube 以外，yt-dlp 仍能轻松应对数千个其它网站——Bilibili、抖音、TikTok、X、PornHub 等等。

即使主战场风云再起，yt-dlp 仍像个斗士，不断突破新的技术壁垒，守护着网络自由的最后阵地。

---

原文：https://www.appinn.com/yt-dlp-returned-from-youtube-crackdown/