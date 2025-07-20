---
title: "一图展示全部信息：提示词 + Figma 十秒精修，让长网页秒变封面（内有白嫖福利）"
date: 2025-07-20
dg-publish: true
---
- description: 教一下大家，如何用提示词生成网页之后再将网页变成对应的图片，而且我还会教你怎么用 Figma 调整生成之后的小问题，导出完美的图片
- source: [url](https://mp.weixin.qq.com/s/uQQ7R8rBUXZ6EoxX4WxMRg)
- author: 歸藏的 AI 工具箱

上周发了个 DeepSeek-Prover-V2 的一图流介绍，一张图展示了 Prover-V2 的主要信息，非常清晰直观，很多朋友都问怎么做的。

![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVa2xqZDcBicm8xoqz8ButJ69Pd8nTj3r618SP1ypxE7faEMrKXbSuqNpQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1)

今天就教一下大家， 如何用提示词生成网页之后再将网页变成对应的图片，而且我还会教你怎么用 Figma 调整生成之后的小问题，导出完美的图片 。

其实这个是从藏师傅的 3.0 网页生成提示词拓展而来的，如果你还没看 3.0 的提示词可以看看《 [藏师傅的网页生成提示词 3.0｜ 原来 Gemini 2.5 Pro 这么强](https://mp.weixin.qq.com/s?__biz=MzU0MDk3NTUxMA==&mid=2247488641&idx=3&sn=a30633a8b302deddb72f2ae46ab16c1a&scene=21#wechat_redirect) 》。

上周 Orange 来找我说用我的 3.0 提示词把刚发布的千问 3 模型内容变成类似苹果发布会 PPT 的一图流展示非常直观。

就是有个问题是生成的网页很长不好截图，我就提了一嘴说让 AI 尽量在一页生成就行，他试了一下果然行，可以看看我生成的 Gork 3 和千问 3 的模型介绍。

![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVak4XmBBNgymsINQ7au3d2ibiaDPicGibribAlKnDxwIQh50ic4LJaIplVGXLQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1) ![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVayTkEcA0f56JzwRdRjpvoUxcNSGMXNib10bZ0sHCntmb8j1ndArUwafw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1)

## 生成网页

我们就以 DeepSeek-Prover-V2 这个例子介绍一下，还是我之前讲 Vibe Coding 说的第一次生成结果至关重要，所以我们需要准备一些东西。

首先是模型的论文或者介绍博客：

如果你要介绍产品更新也是一样，需要上传文档，论文的话就是 PDF 就行，如果是介绍博客就直接全选网页内容保存一个 Markdown 文件就行，也可以用我之前也介绍的 Obsidian 剪藏插件获取。

然后就是输入提示词了：

这里我们只是基于藏师傅网页生成 3.0 提示词上加了一句话“ 尽量在一页展示全部信息 ”。

这里你可以根据模型品牌的主题色更改第一条的颜色部分，比如 Qwen 就是白色背景紫色高亮，Grok 就是暗色背景橙色高亮。

![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVa8015evxRibh1nXcuVj76gW79XZJswLEmcx70tL7V7MdGHDBGUlTM8vA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1)

```
基于模型发布文档的关键信息，帮我用类似苹果发布会PPT的Bento Grid风格的视觉设计生成一个中文动态网页展示，具体要求为：

1. 尽量在一页展示全部信息，背景为白色、文字和按钮颜色为纯黑色，高亮色为#4D6BFE
2. 强调超大字体或数字突出核心要点，画面中有超大视觉元素强调重点，与小元素的比例形成反差
3. 网页需要以响应式兼容更大的显示器宽度比如1920px及以上
4. 中英文混用，中文大字体粗体，英文小字作为点缀
5. 简洁的勾线图形化作为数据可视化或者配图元素
6. 运用高亮色自身透明度渐变制造科技感，但是不同高亮色不要互相渐变
7. 数据可以引用在线的图表组件，样式需要跟主题一致
8. 使用HTML5、TailwindCSS 3.0+（通过CDN引入）和必要的JavaScript
9. 使用专业图标库如Font Awesome或Material Icons（通过CDN引入）
10. 避免使用emoji作为主要图标
11. 不要省略内容要点
```

一般这个时候第一次生的结果就已经不错了，如果没有问题的话你就可以用最大的显示面积截图就行。

但是经常会有一些小问题，比如在布局上可能会出现的问题有标题没有加上卡片和边框，或者在某个部分的卡片没有占满全部的空间，比如我 Deepseek 这个就有问题。

![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVapLCvuIuSWY6UdKWD1kXKrPcyLgeuCkwssjG2py0towdFQYicibZOr2mw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1)

这种问题让 AI 改的话他就会全部生成，而且我们也不好描述，这时候就可以导入到 Figma 我们手动调整一下，可能就十几秒就能搞定。

## Figma 微调设计稿

首先我们需要找到这次要用的核心 Figma 插件 html.to.design 只需要在随便一个 Figma 文件里面点击下方 Tab 栏圈住的图标之后搜索就行。

![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVaMwdjEPia2dVz042nsaY4MhScMbbnpFiaR9y8d2M4n2vVFL6OiaxI1Ahog/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1)

然后我们输入需要导入的网页地址，如果你没有的话可以用 Youware 部署，然后点击 Import 按钮就行.

之后导入一般的之后他会让你选一下其他选项，这里除了不让开的开关你都开了就行，然后 Font Mappings 字体设置这里需要选替换字体，如果你的网页有中文的话我推荐你换成 Pingfang SC。

![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVadHQpjTUTRFhdibtBMJVAiaJ8eZKZuv9tRMBdibreKn9p84Vm1sPoXtzbA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1) ![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVa2sM9SkueA5JPHgL0tMLTaqSNIhD9YhUtegK6bUKL6wiaXVhJCkibU8PA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1)

然后你就能看到导入的网页了，是一个完整的画板，里面还有 Youware 的下导航这里有很多我们不需要的部分，你就可以直接选中这部分删掉就行。

![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVaNceq9g5g8W3jo7ytyE6PwicDiciaEzotTDPb7Ukkicf3L7xOEexnRtfKBw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1)

删掉没用的 tab 栏之后我们发现 Iframe 这个里面也没有内容，然后实际的内容都在红框没有权重的那个 Iframe 里面。

所以我们需要删掉最下面的Iframe图层，之后讲 Container 和 1920w light 取消编组，这样我们就剩下了真正有用的部分。

![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVacwibuRlRo0GiapyT9ibV3ruZuL9EJnqWpickTf3bJODwgsryFf6r1ueIpA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1)

终于要进行我们的改动了，首先我们希望给头部的标题也加上卡片，这时候我们选了一下发现头部 Header 的宽度比下面所有卡片加起来的宽度是要宽的所以先把他们的宽度统一改成 1472px。

![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVa33MBq21523VFjQfpPCTeoicB7sbPrd4ASwriaROw3YianZDE3ica1HQE8Q/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1)

之后我们想要复制下面灰色卡片的样式而不需要他的内容，只需要随便选一个下面的灰色卡片，然后右键-复制粘贴为-复制属性就行，粘贴也是一样选择上么的 Header 卡片选择粘贴属性，你就发现标题也有卡片了。

![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVaTm186gACv5qAwhlA4ibwOOpeKMMbLKvTdqyEQkwQ7UNIJGkjGmaeYFw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1)

接下来我们修复，模型规模这里的卡片没有占满全部空间的问题，选中模型规模的卡片，按住 Option 按钮我们发现他到双模训练卡片的宽度是 398 然后他们需要有 24px 的间距。

![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVa7nnJdXOSyRHAicpXW1JTjTIvmUxTicPDKs6cy22ibgK7o3efRALKxoSug/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1)

所以模型规模的卡片宽度应该为 350+398-24，你直接在宽度输入框写数学公式就行，Figma 会帮你算好的，现在是不是 OK 了。

![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVahBkEFO2EicZD8rfw7SibYDSrIicBBPsrlfsmbEJ6icdvApJOLTY1icdD8DA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1)

最后我们做导出前的最后一步，整个卡片四周的边距有些问题，左右很宽上下很窄，我们想要他们一样，这个时候我们只需要选中 html-Body 这个图层，然后把红框部分的间距都改成统一的 32 就行。

之后将html-Body 宽度这里改成适应内容。

![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVazicN4A1wtoEbh9uuib9sMkeb6y1z8FIUIU1khRW9N5hCAicBtbRc0yUsg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1) ![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVaFVw6IQjfLjPS7Uot514Rf2icAYicyrUoHIAe53rXu8v89cj9Tr5jS3Rw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1)

最后将最上面的 Iframe 画框右键取消编组，你会发现已经搞定了，然后我们选中这个画板，把最右侧拉到底部在导出这里点一下加号，然后点击导出按钮就行。

![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVaU3OPOH5ctNXr5ZVSBD5ufeHg6qsDR32NnNVhcLUrz7OTibB0EPGMRKg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1)

如果你想要想我上面的的展示图片那样给图片加个渐变边框的话可以用 postspark（ https://postspark.app/screenshot ） 这类工具。

![Image](https://mmbiz.qpic.cn/mmbiz_png/fbRX0iaT8EgfOx02QqTJRdZEyb1OtOpVa5Ch5gh6caGLz9kfNCDPQ4rtN53EM2VluCibluxic8C3RTGcgWIACsgxg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1)