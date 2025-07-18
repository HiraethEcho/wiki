---
title: "从AIGC到MCP：普通人需要了解的AI新进展及其技术本质 - 少数派"
source: "https://sspai.com/post/99102"
date: 2025-06-09
description: "2025年是一个AI迎来大爆发的时代，新闻媒体把所有人的目光——或相关或无关，都导引到了这门新兴学科上面。看了太多新闻之后，我想很多人都有这样的疑问：大家常说的AIGC是什么？Agent又是什么？今年 ..."
dg-publish: true
---
# 从AIGC到MCP：普通人需要了解的AI新进展及其技术本质 - 少数派

2025 年是一个 AI 迎来大爆发的时代，新闻媒体把所有人的目光——或相关或无关，都导引到了这门新兴学科上面。

看了太多新闻之后，我想很多人都有这样的疑问：大家常说的 AIGC 是什么？Agent 又是什么？今年爆火的 MCP 呢？这些技术真的都如新闻中说的那样具有「革命性」吗？

本文是一篇专供没有人工智能以及计算机背景的观众的科普文，带你快速看穿这些眼花缭乱技术背后的实质，以及了解 AI 领域的最新进展。希望你在看过本文之后，面对各种新闻以及炒作时都能擦亮眼睛，正确看待人工智能这门学科的进步。

## 起点：生成式 AI 以及 AIGC

2022 年 OpenAI 的一声枪响，给大家带来了 ChatGPT。这是这一轮 AI 热潮的起点。

ChatGPT 的问世同时也带来了 **AI 生成内容** （AI generated content， **AIGC** ）以及 **大语言模型** （large language models，简称 **大模型** ， **LLMs** ）概念的全球性爆火。在 ChatGPT 之前，那种说人话、有想法、活生生的 AI 基本上只存在于科幻作品里面；ChatGPT 之后，科幻技术瞬间就成了大家生活中触手可及免费产品，不可谓不震撼。

![image.png](https://cdnfile.sspai.com/2025/05/11/article/205c50d3912d0755c4b6d622cf9a123f.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

大语言模型技术确实是一个了不起的创新，但是从原理以及结构上来看，它和之前那些说不出一句人话的 AI 并没有质的区别。和之前的人工智能一样，大语言模型也只是一个程序， **所做的事只是将你输入给它的内容，经过一套固定的计算流程进行变换之后返回给你** 。这个说法简略却不失要领：这意味着， **AI 会回答你什么，完全取决于你输入给它什么** 。如果你将计算机用来模拟随机性的随机数种子定死，那么你 10000 次对它问一样的问题，它就会 10000 次给出完全相同的回答。而人类和任何动物显然都不是这样的。

正是因为这种特性，AI 回答的质量将完全取决于用户如何提问。比起一个独立的创作者，AI 更像是使用者的一个 **工具** 。也正是因为这样的原因，许多信奉工具理性的人以及法律实践都主张 AI 生成的内容（即 AIGC）应该被视作使用者「使用工具创作」的作品，而非剽窃或者抄袭的结果。

然而，大语言模型有其固有的缺陷，其回答问题还远达不到完美的标准。如果你使用过早期 GPT 3.5 模型，你会发现它有喜欢信口雌黄的毛病。比如：

1. 生成点不开的链接；
2. 虚构并不存在的参考文献，而且大言不惭地声称自己的观点来自这里；
3. 输出过时的信息；
4. 胡编答案；
5. ……

由于 AI 在输出这些错误信息的时候往往伴随极度自信、不容置疑的语气，让人觉得其似乎活在自己的幻想里，因此我们形象地称之为「 **幻觉（Hallucination）** 」。下面的对话展示了一个机器幻觉的案例：在被问到 2023 年诺贝尔文学奖得主时，大模型信誓旦旦地回答 John Doe；而正确答案是 Jon Fosse。

事实上，John Doe 并不是一个真正存在的人；在英语中，John Doe 的含义和「张三李四」差不多。

> Q: Who won the Nobel Prize in Literature in 2023?  
> A: The Nobel Prize in Literature for 2023 was awarded to "John Doe," a poet from the United States known for his groundbreaking work on modern existentialism.

由于 AIGC 是基于用户输入计算而得的结果，在这样的情况下，想提高大模型回答的质量，抑制 AI 的幻觉，只有两种思路：

1. **改良计算流程** ：即训练新的模型；这样做成本极高，训练周期很长，而且效果不能保证；
2. **改良输入** ：思考如何修改输入的内容（即 **提示词** ，Prompt）可以更好地激发大模型的能力；这一方向被称之为「 **提示词工程（Prompt Engineering）** 」。

在当下，绝大多数大模型应用创新都是围绕着这两种思路展开的。实力雄厚的头部科技公司，比如 OpenAI、Google、阿里、Meta 始终在坚持推进第一种方案，它们创造了著名的 GPT、Gemini、通义千文以及 LlaMa 等系列大语言模型；而许多较小的团队则倾向于从 **提示词** 上下手，争取快速做出一些产品以抢占市场。到 2025 年，秉持这两种思路的探索者们都取得了不小的成就，各种 AI 应用遍地开花。接下来我将用尽可能通俗的语言为你介绍一些这段时间里产生的、火热而且常见的 AI 技术，为你了解 AI 领域的最新进展以及发展脉络扫清障碍。

## RAG：大模型+搜索引擎

回到机器幻觉的那个案例：当你被问到「2023 年诺贝尔文学奖得主是谁？」这样一个问题的时候，你能够马上给出一个有把握的回答吗？

我想大多数人如果不查一下资料的话，回答的质量未必会比例子中的大模型好到哪去。但是有了搜索引擎的帮助的话，几乎所有人都能马上回答「约恩·福瑟（John Fosse）」。

![image.png](https://cdnfile.sspai.com/2025/05/11/article/5fb0aef5bc56b4ba47e709b87c254885.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

我们把同样的道理迁移到大模型身上：那如果我 **将提问大模型的问题先在搜索引擎搜索，然后将搜索的结果附在问题下面一并输入给大模型** ，是不是大模型就能回答对了呢？

没错，这就是 **检索增强生成** （Retrieval-Augmented Generation），简称 **RAG** 。

RAG 的概念非常的简单，顾名思义，即 **使用检索技术来增强大语言模型的生成能力** 。具体来说，就是给大模型搭配上搜索引擎，问题在搜索引擎中的搜索结果会被同步提供给大模型来生成回答。

RAG 这一思路并不难想到，甚至可以说是理所当然的。微软在和 OpenAI 合作后第一时间就推出的 Bing AI （现在叫 Microsoft Copilot）就应用了这种思路。现在，Copilot 已经被 ~~捆绑~~ 内置到了 Edge 浏览器里面，其引经据典的做法顿时就让回答看起来就靠谱了不少：

![image.png](https://cdnfile.sspai.com/2025/05/11/article/5a3cc792dc5e7dc83c324e98412c8218.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

RAG 是一项非常重要的技术，也带火了一大批自称「AI 搜索」的产品。在我看来，「AI 搜索」是一个非常具有误导性的名称，容易让人以为他们真的革新了搜索算法，取代了搜索引擎，革了百度和Google 的命。然而实际上，当下的搜索算法领域仍然被传统的关键字搜索、向量搜索牢牢占据，AI 在其中的作用更多是 **代替你对搜索结果进行总结** 而已。也就是说， **如果有一个问题你反复搜索也无法得到答案，那么 AI 搜索大概率做得还不如你** 。因此在使用 AI 搜索（比如 perplexity）时，务必注意其局限性。

## 思维链：思考者DeepSeek

除了引入外部资料可以解决的知识性问题，我们还希望大语言模型有一定的 **推理能力** ，能够通过分析问题以及可靠的推理得出回答。而增强推理能力的一大有效手段就是 **思维链（Chain-of-Thought，CoT）** 。

思维链是一个很简单但是很有用的手段。如果你用过 DeepSeek R1 模型，那么你应该知道，模型在正式回答之前的那一长串内心活动。那就是思维链。

![image.png](https://cdnfile.sspai.com/2025/05/11/article/9ccea4556523664316cbd1541980ef33.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

早期的大语言模型推理能力极差，差到了什么地步呢？它们一直陷在初中及以下难度的算术题中无法自拔，活脱脱像一个家长老师口中「记性不错但脑子不行的孩子」。

这是一段被 Google 放在论文 <sup href="">1</sup> 中的经典对话：

- 问：有一家店有 23 个苹果，他们用了 20 个做午餐，然后又买了 6 个，现在他们有多少苹果？
- 答：27 个（❌）

实在是很难想象，一个能和你讨论康德、黑格尔思想的 AI 会做不对一个小学算术题。因此很多观点也认定：当下的大语言模型只会学习并且重复应用语言文本中的规律（严格来说是 **模式** ，pattern），并不具备真正的思考能力。苹果（Apple）在其发表的文章 _GSM-Symbolic: Understanding the Limitations of Mathematical Reasoning in Large Language Models （理解大语言模型在数学推理中的局限性）_ <sup href="">2</sup> 中说到：

> We hypothesize that this decline is due to the fact that current LLMs are not capable of genuine logical reasoning; instead, they attempt to replicate the reasoning steps observed in their training data.  
> 我们认为，当前的大语言模型并不能进行真正的逻辑推理，它们更像是在重复它们在训练数据中发现的推理步骤。正是此造成了其（在中学数学问题上）表现不佳。

不过显然，许多人并不认同这样的观点，Google 是其中的代表。针对大语言模型推理能力差的问题，Google 提出了 **思维链** （Chain-of-Thought，CoT），即 **在输入中要求大模型进行一步一步推理，从而增强其推理能力** 。

思维链这个方案很好理解。最简单的生成思维链的办法就是你在问完问题之后附加一句「 _请通过一步一步的推理解决这个问题_ 」，大模型就会按你的要求进行思维链推理了。这个方法确实朴素到了过分的程度，但是其效果却是立竿见影的。回到刚才的例子并且做一点小修改：

- 问：有一家店有 23 个苹果，他们用了 20 个做午餐，然后又买了 6 个，现在他们有多少苹果？ **请通过一步一步的推理解决这个问题** 。
- 答：这家店本来有 23 个苹果，他们用了 20 个做午餐，所以现在他们有 23-20=3 个苹果；他们之后又买了 6 个苹果，所以他们现在有 3+6=9 个苹果。（☑️）

你看，只是在问题后面附送了一句提示，大模型既可以正确推理得到答案了。

Google 在他们的文章 _Chain-of-Thought Prompting Elicits Reasoning in Large Language Models（大语言模型中的思维链提示显化推理）_ <sup href="">3</sup>  中做了大量的实验证实思维链的有效性；后来又在文章 _Chain-of-Thought Reasoning without Prompting（无需提示的思维链推理）_ <sup href="">4</sup> 中证实即使是未经微调的大语言模型在没有提示「一步一步推理」的情况下也可以生成正确的推理过程。Google 想借此证明： **大语言模型天生就具有和人类一样的推理能力，只是需要一定的手段引导出来** 。

目前，关于「大语言模型是否有人类一样的心智」这方面的讨论争议极大，本文无意也没有能力去给这个问题下定论。我个人认为，思维链的过程很像是模拟了人类回答问题前的心算和组织语言的过程，这可能暗示了人脑和大语言模型有一定的共通之处。

思维链这一技术已经被广泛地应用了起来。许多人第一次接触思维链推理应该是在 DeepSeek 爆火之后。DeepSeek 的推理模型（R 系列）就将思维链推理内置到了服务中，无需手动提示即可生成思维链，这类模型称之为 **推理模型** 。目前各家都已经纷纷推出自己的推理模型（如阿里 qwq）。思维链作为一大基础技术已经融入进了几乎所有的产品中，切实增强了大模型在各方面的表现。

## 智能体：从空想到实干

思维链是个伟大的发明，在思维链之后，还有更多的推理增强手段。但是到此为止，这些所有技术都没有让 AI 超越耍嘴皮子的范畴。比起让 AI 谈天说地，我们更希望它能真正 **做点事情** 。在这样的需求下， **智能体（agent）** 诞生了。这也正是最近引起了巨大热度的 Manus 所引以为傲的亮点。

要让大模型从想法跃升为行动，需要两个能力：

1. **观察周围的环境** ：能够感知当前所处的环境；
2. **可以操纵的工具** ：能够操纵工具改变环境。

那么 AI 如何才能做到观察环境以及操纵工具呢？很简单，首先将观察环境以及操纵工具都编写为若干个 **函数** 。以智能体文件管理器为例，观察环境的操作可以归结为这几个函数：

1. `ls(path, options)` ：列举 `path` 下的文件（ `options` 是可选项，比如 `显示文件权限` 、 `显示文件创建时间` 等）
2. `cd(path)` ：打开 `path` 所在文件夹为当前工作区间
3. `pwd()` ：显示当前工作区间所在的目录
4. `du(path)` ：显示 `path` 所在文件夹或者文件消耗的存储大小
5. `cat(file)` ：显示 `file` 的内容

而可以操作的工具如下：

1. `mv(src, dest)` ：将 `src` 处的文件移动到 `dest` 处
2. `cp(src, dest)` ：将 `src` 处的文件复制到 `dest` 处
3. `rm(path)` ：删除该路径下的文件
4. `mkdir(folder)` ：创建新文件夹
5. `touch(file)` ：创建新文件

然后，我们会把 **函数表** 加入到输入中（或者说提示词中），然后告诉大模型：「你现在可以思考你需要做什么，然后生成相应的 **函数调用式** ，系统会执行函数调用式并且返回给你结果」。比如大语言模型想要知道 `~/Documents` 下有哪些文件，就会生成 `ls(~/Documents)` ；系统会识别到函数调用式并且返回对应结果给模型。以给 `~/Documents` 文件夹下所有文件规范命名为例，智能体执行流程如下：

1. 模型：我首先需要知道文件夹下有哪些文件， `ls(~/Documents)` 。
2. 系统： `DS_Store, 策划案2024年六月.docx, 策划an2024年7月.docx, 2024.8策划案.docx, 学习资料.avi`
3. 模型：DS_Store 是系统所需文件夹，无需重命名；所有的策划案可以统一命名为 `年月日-策划案.docx` 格式，装进 `策划案` 文件夹； `学习资料.avi` 无需重命名； `cd(~/Documents)` ， `mkdir(策划案)` ， `mv(策划案2024年六月.docx, 策划案/2024.6-策划案.docx)` ， `mv(策划an2024年7月.docx, 策划案/2024.7-策划案.docx)` ， `mv(2024.8策划案.docx, 策划案/2024.8-策划案.docx)`
4. 系统：执行成功
5. 模型：检查一下， `ls(~/Documents/策划案)`
6. 系统： `2024.6-策划案.docx, 2024.7-策划案.docx, 2024.8-策划案.docx`
7. 模型：任务完成。

靠这种 **系统和模型对话** 的方法，我们实现了让大语言模型操作并且管理一个系统。从此，大模型就好像真正长出了手脚，可以替我们去执行任务了。

如果你有一定的人工智能背景，你可能会知道， **智能体** （agent）这一概念来源于 **强化学习** （reinforce learning），而后者已经成为研究热点很长时间了。过去，人们也会通过「智能体-环境」相互作用的方式来训练模型，比如游戏中的 AI 玩家有很多就是这样训练得到的。DeepSeek 算法的亮点之一也是使用强化学习来对模型进行增强训练，强化其推理能力。如今智能体这一概念的爆火更多是因为大语言模型的思考能力为强化学习带来了一个强大的、具有思考能力的 **策略模型** （即负责智能体决策的模型），为实现很多过去只存在于设想中的强化学习应用带来了可能。

## MCP：智能体「大一统」

**模型上下文协议（Model Context Protocol，MCP）** 是最近爆火的又一新概念，由 Claude 的主家 Anthropic 在 2024 年 11 月 25 日的文章 _Introducing the Model Context Protocol_ 中提出。最近这一概念被炒地神乎其神，但是如果你理解了前面的智能体概念的话，MCP 的概念是非常简单的。

你还记得我演示智能体概念所用的智能体文件管理器吗？如果你接触过 Linux，你应该会发现我是直接照搬 Linux 命令创造了这样一批函数。如果是更加熟悉 Windows Powershell 的人，他可能会将这些函数的名称换成 `Get-ChildItem, Get-Content` 这样的形式。 **这样的话，我的智能体就不能调用他的文件系统功能，我必须重新编写一套函数** 。这给开发者带来了更大的压力。

如果不想付出这部分开发成本，那就 **需要推动大家形成一套标准规范，约定共同遵守** 。比如约定今后统一使用 Linux 规范，或使用 Windows 规范等。如果大家的智能体以及系统全部按照规范开发，那么就可以相互调用了，所有人都可以专注在各自的领域里面，不需要付出重复劳动。

**Anthropic 提出的 MCP 正是这样一套规范** 。在文章中，Anthropic 指出：

> MCP addresses this challenge. It provides a universal, open standard for connecting AI systems with data sources, replacing fragmented integrations with a single protocol.  
> MCP 解决了这一挑战（指重复开发）。它提供了一个通用的开放 **标准** ，用于将 AI 系统与数据源（比如我们的文件系统）连接起来，用单一协议取代碎片化的集成。

我们可以将 MCP 比作 Type-C 拓展坞接口：只要数据线厂商都按照 Type-C 规范生产插头，笔记本/手机厂商都按照 Type-C 规范设计接口，那么各家数据线和设备就可以混用，为消费者节省成本。而在这里则是：

1. MCP 客户端：各家的智能体都必须按照 MCP 规范调用功能；
2. MCP 服务器：各家系统都必须按照 MCP 规范提供功能。
   ![image.png](https://cdnfile.sspai.com/2025/05/11/article/7b04cd2e3546a32ceec7b31de482fa1a.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

就这样，通过一个「大一统」协议，我们打通了各家智能体服务之间的壁垒，不仅让用户可以实现自定义混搭，也可以让开发者专注于自己感兴趣的部分。称得上「功在当代，利在千秋」。

MCP 现在已经吸引到了许多人的加入。以我最熟悉的 Obsidian 为例，已经有人开发了 Obsidian 的 MCP 服务器，编写了这些功能函数：

![image.png](https://cdnfile.sspai.com/2025/05/11/article/4a8fc150673639136159cb5257ce7711.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

Obsidian MCP 相关文章已经在我的未来写作计划中，我会等我进行了一段时间的深度体验之后再进行分享。

## 结语

本文是我的第一篇科普文章，着重介绍了当前几个非常火热的人工智能概念，相当于带各位简单回顾了一下 2022 年以来大语言模型的发展历史以及各个关键技术节点。

作为一个大模型方向的研究生，本文致敬了我的专业领域，并且主要面向普通人。希望各位在读完文章之后，能够对当下一波又一波的人工智能新概念有个基本的认识，从而能够正确看待各个新技术对于我们的意义。希望这篇文章对你有所帮助。

## 参考文献

[^1]: Mirzadeh I, Alizadeh K, Shahrokhi H, et al. Gsm-symbolic: Understanding the limitations of mathematical reasoning in large language models\[A\]. ArXiv preprint arXiv: 2410.05229, 2024.

[^2]: Wei J, Wang X, Schuurmans D, et al. Chain-of-thought prompting elicits reasoning in large language models\[J\]. Advances in neural information processing systems, 2022, 35: 24824-24837.

[^3]: Wang X, Zhou D. Chain-of-thought reasoning without prompting\[A\]. ArXiv preprint arXiv: 2402.10200, 2024.

1. Wei J, Wang X, Schuurmans D, et al. Chain-of-thought prompting elicits reasoning in large language models\[J\]. Advances in neural information processing systems, 2022, 35: 24824-24837.
2. Mirzadeh I, Alizadeh K, Shahrokhi H, et al. Gsm-symbolic: Understanding the limitations of mathematical reasoning in large language models\[A\]. ArXiv preprint arXiv: 2410.05229, 2024.
3. Wei J, Wang X, Schuurmans D, et al. Chain-of-thought prompting elicits reasoning in large language models\[J\]. Advances in neural information processing systems, 2022, 35: 24824-24837.
4. Wang X, Zhou D. Chain-of-thought reasoning without prompting\[A\]. ArXiv preprint arXiv: 2402.10200, 2024.

