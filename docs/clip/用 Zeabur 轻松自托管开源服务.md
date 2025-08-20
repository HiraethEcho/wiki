---
title: "优惠上架 | 用 Zeabur 轻松自托管开源服务 - 少数派"
date: 2025-08-20
dg-publish: true
---
- description: 今年早些时候，我们与新兴云端部署服务Zeabur合作，举办了一次「自力更生」征文活动。如我们在发起活动时所说，虽然主流服务越发被个别巨头所垄断，但自托管（self-hosting）——在自己的服务器或 ...
- source: [url](https://sspai.com/post/93669)
- author: 福利派

今年早些时候，我们与新兴云端部署服务 [Zeabur](https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2F) 合作，举办了一次「自力更生」征文活动。

如我们在发起活动时所说，虽然主流服务越发被个别巨头所垄断，但自托管（self-hosting）——在自己的服务器或设备上运行、管理软件或服务——作为一种实践并没有消亡，反而选择更多、门槛更低了。我们的目的也是希望能给对自托管感兴趣的朋友一个更好的表达机会，让更多可能受益于自托管的朋友认识这种途径。

那次活动获得了很好的反响。几十篇 [投稿文章](https://sspai.com/post/90350) 有的介绍 NAS 玩机心得，有的分享了推荐的自托管软件，还有的则讨论了自托管过程中遇到过的挑战和解决方案，以及如何平衡自托管中的安全与便利。

不过，搭建一台自己的服务器，从头开始配置自托管服务，对于不少朋友可能还是有一定门槛。如果能像从 App Store 挑选应用那样简单地安装、管理开源项目，并且使得成本相对透明、可控、合理，相信能让更多人有机会、有动力感受到开源软件和自托管的优势。

因此，我们很高兴与 Zeabur 继续合作，为少数派用户介绍 Zeabur 的「一键化」自托管方案，并提供更加优惠的价格。

![](https://cdnfile.sspai.com/11/7/2024/article/d15e9e11-29de-b72e-da9b-206b13caeeb8.png?imageMogr2/auto-orient/thumbnail/!200x200r/gravity/center/crop/200x200/ignore-error/1)

Zeabur 开发者方案

部署服务从未如此简单

¥35.00

## 如何用 Zeabur 一键部署开源服务

Zeabur 上的一键部署是从「模板」开始的。Zeabur 的 [官方模板市场](https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Fzh-CN%2Ftemplates) 已经涵盖了相当数量的流行开源项目，直接搜索使用即可。

![](https://cdnfile.sspai.com/2024/11/07/article/8c0a962d34273f9eb6c644b9c9f28d15.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

下面，我们以安装流行的笔记服务 [Memos](https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2Fusememos%2Fmemos) 为例来说明用 Zeabur 托管的简易步骤。

第一步，通过搜索找到 Memos 的 [Zeabur 模板页面](https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Fzh-CN%2Ftemplates%2FKIJROJ) ，点击「部署」按钮。

第二步，在部署地区选择一个合适的选项，然后选择「确定」，稍等几秒直到显示「服务已就绪」。

![](https://cdnfile.sspai.com/2024/11/07/article/74170ae5a5fec7074d6fdb9be855750e.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

没有第三步了。这时点击「前往控制台」按钮，就可以看到服务的运行状态、资源用量等信息。

如何访问这个新部署的服务呢？在控制台展开「网络」，在这里即可设置一个免费的 `zeabur.app` 子域名。你也可以绑定一个自己的域名，然后在域名托管商的 DNS 解析设置中增加一条 CNAME 记录即可。

![](https://cdnfile.sspai.com/2024/11/07/article/64dcfd597a8a2638848479a789411bf9.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

**注意：** 如果选择国内部署，则需要绑定自己域名，并且你应当根据服务性质，自行完成 ICP 备案（如需）等合规要求。

设置好域名后，直接访问即可用上部署好的服务了。

![](https://cdnfile.sspai.com/2024/11/07/article/8b69b8e38258611c7f01d824459354bf.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

当然，Zeabur 的适用范围远不限于官方模板。由于模板本质上只一个记录了搭建服务相关资源和配置的文件，如果你的动手能力比较强，可以自己为任何开源项目 [制作模板](https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Fdocs%2Fzh-CN%2Ftemplate%2Fcreate-template) 。

在 GitHub 上，有些项目也已经主动适配了 Zeabur，如果你在浏览开源项目 README 的时候看到了下面这样的按钮，点击一下即可部署到自己的 Zeabur 账户。

![](https://cdnfile.sspai.com/2024/11/07/article/7e40a1bbd7e188be97d2ef004bfca07d.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

## 透明、可控的成本

除了技术门槛，成本问题是很多用户转向自托管前的另一个主要顾虑。如果自己购买服务器搭建，配置选高了会闲置，选少了日后又不方便升级（或者成本较高）。

而这也是 Zeabur 的另一个优势所在。Zeabur 的付费模式非常简单：托管 serverless（无驻留服务，例如静态博客）的项目是完全免费的。如果要托管非静态服务，则需要升级到「开发者方案」，价格是 5 美元或 35 元人民币。

在开发者方案下，用户可以部署不限数量的服务，这些服务共享每月 5 美元的资源额度。参与计费的资源包括 CPU 时间、内存、出口流量和持久存储，如下图所示：

![](https://cdnfile.sspai.com/2024/11/07/article/754b28377d658e64d11869980469c212.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

只要部署服务的合计成本在 5 美元之内，就无需额外付费；如果超出 5 美元，支付超出的部分即可。实际上，在个人日常使用强度下，博客、笔记等常见服务的每月开销是相对低廉的，完全可以被每月的固定额度所涵盖。

下表是 Zeabur 官方提供的部分常见开源服务的每月基础成本（指仅安装并维持运行的成本，实际成本会因个人用量而不同幅度地高出）：

<table><tbody><tr><td colspan="1" rowspan="1"><strong>软件名称</strong></td><td colspan="1" rowspan="1"><strong>基础成本</strong></td><td colspan="1" rowspan="1"><strong>相关链接</strong></td></tr><tr><td colspan="1" rowspan="1">Vaultwarden</td><td colspan="1" rowspan="1">$0.27/月</td><td colspan="1" rowspan="1"><a href="https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Fzh-CN%2Ftemplates%2F6CRDA2">Zeabur 模板</a> | <a href="https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2Fdani-garcia%2Fvaultwarden">GitHub</a></td></tr><tr><td colspan="1" rowspan="1">Memos</td><td colspan="1" rowspan="1">$0.9/月</td><td colspan="1" rowspan="1"><a href="https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Fzh-CN%2Ftemplates%2FKIJROJ">Zeabur 模板</a> | <a href="https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2Fdani-garcia%2Fvaultwarden">GitHub</a></td></tr><tr><td colspan="1" rowspan="1">bark-server</td><td colspan="1" rowspan="1">$0.9/月</td><td colspan="1" rowspan="1"><a href="https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Ftemplates%2FD7E65G">Zeabur 模板</a> | <a href="https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2FFinb%2Fbark-server">GitHub</a></td></tr><tr><td colspan="1" rowspan="1">code-server</td><td colspan="1" rowspan="1">$0.9/月</td><td colspan="1" rowspan="1"><a href="https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Ftemplates%2F5S3GX6">Zeabur 模板</a> | <a href="https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2FFinb%2Fbark-server">GitHub</a></td></tr><tr><td colspan="1" rowspan="1">n8n</td><td colspan="1" rowspan="1">$2.1/月</td><td colspan="1" rowspan="1"><a href="https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Ftemplates%2FW2H4RW">Zeabur 模板</a> | <a href="https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2Fn8n-io%2Fn8n">GitHub</a></td></tr><tr><td colspan="1" rowspan="1">AFFiNE</td><td colspan="1" rowspan="1">$2.7/月</td><td colspan="1" rowspan="1"><a href="https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Ftemplates%2FGSZA1K">Zeabur 模板</a> | <a href="https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2Ftoeverything%2FAFFiNE">GitHub</a></td></tr><tr><td colspan="1" rowspan="1">Nextcloud</td><td colspan="1" rowspan="1">$2.75/月</td><td colspan="1" rowspan="1"><a href="https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Ftemplates%2F8UTLCY">Zeabur 模板</a> | <a href="https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2Fnextcloud%2Fserver">GitHub</a></td></tr><tr><td colspan="1" rowspan="1">Linkwarden</td><td colspan="1" rowspan="1">$3/月</td><td colspan="1" rowspan="1"><a href="https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Ftemplates%2F1E5ABT">Zeabur 模板</a> | <a href="https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2Flinkwarden%2Flinkwarden">GitHub</a></td></tr></tbody></table>

可见，对于个人使用场景，开发者方案的每月额度可以用于运行 2 到 3 个轻量服务，或者一个中等规模的服务搭配一个轻量服务，并且仍有余裕——这与单独订阅类似功能的商业服务相比是有竞争力的。而与通过同等价位的 VPS 托管相比，Zeabur 则具有更易管理、计算资源弹性更大、可选择包括国内在内多个部署地区等优势。

当然，具体的费用情况因项目和用法而异，但你可以根据控制台精确到天的统计实时监控并灵活调整。

![](https://cdnfile.sspai.com/2024/11/07/article/059f7fee600056a6d1a1a27527461293.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

此外，你还可以针对特定项目设置预算，Zeabur 会在用量达到预算 80% 时邮件提醒，在达到 100% 自动暂停，以避免意外费用。

![](https://cdnfile.sspai.com/2024/11/07/article/dabf494a51e500526bc8430cc73bb895.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

更多关于计费的说明可以参阅 Zeabur 的 [官方文档](https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Fdocs%2Fzh-CN%2Fbilling%2Fpricing) 。

## 更多值得探索的玩法

除了使用简便、成本透明外，Zeabur 还有很多值得探索的特点：

- 支持「自带」服务器：你可以将自己拥有的服务器 [设置为](https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Fdocs%2Fzh-CN%2Fdedicated-server) Zeabur 的运行环境，运行在自己服务器上的项目不会收取资源费用。这样，既能享受一键部署的快捷，又能充分利用现有的硬件资源。
- 自动化集成：Zeabur 有完善的 [命令行工具](https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Fdocs%2Fzh-CN%2Fdeploy%2Fdeploy-in-cli) 和 [API](https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Fdocs%2Fzh-CN%2Fdeveloper%2Fpublic-api) ，高级用户可以将其集成到自己习惯的自动化方案中。
- 核心组件开源：作为一个与开源社区深度互动的产品，Zeabur 用于分析和构建项目的核心组件 `zbpack` 也是 [自由软件](https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2Fzeabur%2Fzbpack) （MPL 2.0 许可），有能力的朋友可以随意审阅、备份、借鉴。 `zbpack` 的更多 [技术细节](https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Fdocs%2Fzh-CN%2Fadvanced%2Fbuilds) 见于官方文档。

……

我们鼓励你通过 Zeabur 的 [官方文档](https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Fdocs%2Fzh-CN) 和 [社区](https://sspai.com/link?target=https%3A%2F%2Fzeabur.com%2Fdocs%2Fzh-CN%2Fcommunity%2Fverify) 进一步了解和交流，也欢迎在少数派分享你的使用心得。

## 在少数派获得独家优惠

Zeabur 即日起将上架少数派软件商城，并为少数派会员提供优惠价格。双十一活动（即日起至 11 月 30 日）期间购买还可享受进一步折扣：

<table><tbody><tr><td colspan="1" rowspan="1"><strong>规格</strong></td><td colspan="1" rowspan="1"><strong>常规价</strong></td><td colspan="1" rowspan="1"><strong>双十一优惠价</strong></td></tr><tr><td colspan="1" rowspan="1">$5 额度<br>相当于 1 个月开发者方案*</td><td colspan="1" rowspan="1">35 元<br>少数派会员：30 元</td><td colspan="1" rowspan="1">30 元</td></tr><tr><td colspan="1" rowspan="1">$60 额度<br>相当于 1 年开发者方案*</td><td colspan="1" rowspan="1">400 元<br>少数派会员：360 元</td><td colspan="1" rowspan="1">360 元<br>少数派会员：320 元<br>（ <a href="https://sspai.com/prime/story/member-2024-double11-promo">领取优惠券</a> ）</td></tr></tbody></table>

\* 本产品以兑换码的形式交付，兑换后将在账户充入相应余额。此处标注的时长是指在使用期间，每月均不超出开发者方案随附 $5 资源费用的情况下，上述余额可以用于充值开发者方案的持续时间；开发者方案的费用将在每月账单日从余额中自动扣除。如产生额外资源费用，余额将优先用于抵扣额外费用，实际可用于持续支付开发者方案的时长也将变短。

![](https://cdnfile.sspai.com/11/7/2024/article/d15e9e11-29de-b72e-da9b-206b13caeeb8.png?imageMogr2/auto-orient/thumbnail/!200x200r/gravity/center/crop/200x200/ignore-error/1)

Zeabur 开发者方案

部署服务从未如此简单

¥35.00

本文责编：@ [PlatyHsu](https://sspai.com/u/platyhsu)

© 本文著作权归作者所有，并授权少数派独家使用，未经少数派许可，不得转载使用。

请在 后评论...[PeixyJ](https://sspai.com/u/sy3299c7/updates)

开发者一个月 5 美金≈35 元,一年的便宜阿里云才 100 块...[Yuanlin](https://sspai.com/u/yuanlin/updates)

哥们你说的没错，幸好 Zeabur 也支持绑定阿里云服务器，如果觉得 5 美金太贵的话自己去租便宜的 vps 绑定到 Zeabur 上也是可以的呢 👍[Clouder](https://sspai.com/u/clouder/updates)

selfhost 的一大目标是避免生态绑定、掌控自己的数据，这种平台更类似 PaaS 如 heroku/vercel 吧。。。降低 selfhost 部署门槛的方案应该考虑 CasaOS 之类的。