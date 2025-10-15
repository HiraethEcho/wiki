---
title: "从零开始理解 Git｜纯手工打造 Git 仓库｜太长可以不看"
date: 2025-10-15
---

- description: 但实际上，这篇文章不是从零开始，它更像是当你已经使用了很久的 git，熟悉各种操作之后，又想再次重新开始，重新理解 git 是什么的内容。 比如：当你输入了 git init 之后，究竟都发生了什么。
- source: [url](https://www.appinn.com/drew-artisanal-git/)
- author: 青小蛙

很长，可以先收藏，然后不看。

一篇可以很好的“从零开始”理解 Git 的文章。

本文链接： [https://www.appinn.com/drew-artisanal-git/](https://www.appinn.com/drew-artisanal-git/)

![从零开始理解 Git｜纯手工打造 Git 仓库｜太长可以不看 1](https://www.appinn.com/wp-content/uploads/2025/10/Copy-of-appinn-homework-2025-10-15T094114.954.jpg)

从零开始理解 Git｜纯手工打造 Git 仓库｜太长可以不看 1

## 从零开始，太长不看

但实际上，这篇文章不是从零开始，它更像是当你已经使用了很久的 git ，熟悉各种操作之后，又想再次重新开始，重新理解 git 是什么的内容。

### 当你输入了 git init 之后，究竟都发生了什么。

我们使用 `git init` 可以初始化一个仓库，但是 `git init` 到底发生了什么？

1. 创建.git 目录
2. 创建.git/hooks.git/info.git/objects/info.git/objects/pack \\.git/refs/heads.git/refs/remotes.git/refs/tags.git/logs 一大堆目录
3. 创建.git/config 文件并写入一些内容

这篇内容，就是告诉你，输入以下这些命令，究竟都发生了什么：

- git init：手动创建.git 目录和相关文件，代替初始化仓库的命令。
- git status：通过检查.git 目录结构和 HEAD 文件，判断仓库状态。
- git commit：手工构造 blob、tree 和 commit 对象，并创建提交记录，而非直接用命令提交。
- git cat-file：利用命令或二进制工具查看对象内容，但在文中实际是教你如何通过文件结构和解压工具查看对象。
- git diff：说明 Git 实际上不是存储 diff，而是存储完整快照，通过文件内容手工“比对”理解 diff 的原理。
- git branch 和相关引用：手动创建 refs/heads/main 文件来表示分支指向的 commit。
- git reflog：介绍了可以借助.git/logs 手动追踪引用日志，理解 reflog 的机制（虽然没有全部手工实现）。
- git fsck：通过分析错误和目录结构，手动检查仓库完整性和修复问题。

所有的这些，都可以手动、一步一步来实现。

## 真的很长，收藏为主吧。

![从零开始理解 Git｜纯手工打造 Git 仓库｜太长可以不看 2](https://www.appinn.com/wp-content/uploads/2025/10/generated-image-1.avif)

从零开始理解 Git｜纯手工打造 Git 仓库｜太长可以不看 2

- 原文：Artisanal Handcrafted Git Repositories
- 链接： [https://drew.silcock.dev/blog/artisanal-git](https://drew.silcock.dev/blog/artisanal-git)
- 作者：drew

## 手工打造的 Git 仓库

如今我们为 Git 仓库所投入的爱与耐心实在太少了。

那就让我们来改变一下。

这篇文章想聊聊怎样不用那些“傻瓜式”的 git 命令，亲手捏一个 Git 仓库。

顺带的收获是，你可能也会多了解一些 Git 在幕后是怎么运作的，或者别的什么东西。

如果你愿意，也可以趁机感受一下：Git 的力量并不是来自代码的复杂，而是来自它设计上的简洁与优雅。嗯，如果你喜欢这种美学的话。

Git 把那些对人类友好的命令叫作 “porcelain”（瓷器），把内部机制叫作 “plumbing”（管道）。所以你可以把本文当作 Git 管道学入门。事实上，Git 还有不少 “plumbing” 命令我们都用不上，所以这甚至更像是一堂 Git 流体力学速成课。总之，挺滑稽的。

## 先决条件

我假设你已经熟悉 Git，也能自如地在命令行里工作；否则下面这些内容大概都说不通。

## 开始动手

我们平常做的第一件事是运行 `git init` ，但这还有什么匠心可言？我们要走回古法，像久远的朝圣者那样干。

```bash
$ mkdirartisanal-git
$ cdartisanal-git
 

$ mkdir.git
 

$ mkdir-p .git/hooks.git/info.git/objects/info.git/objects/pack\
    .git/refs/heads.git/refs/remotes.git/refs/tags.git/logs
 

$ cat<<'EOF'> .git/config
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true
    ignorecase = true
    precomposeunicode = true
EOF
```

再添上最后一个文件，我们的 Git 仓库就算合格了，那就是 `HEAD` 。你可能见过它—— `HEAD` 代表“仓库此刻指向哪里”。它只是一个文本文件，要么写着某个引用（仓库的常态），要么直接写着提交哈希——后者就是所谓的 “detached head”（听起来就很头疼，对吧）。

我们要让 Git 指向默认分支，也就是 `main` 。

```bash
$ echo"ref: refs/heads/main"> .git/HEAD
```

你可能会想——等一下， `main` 分支在哪？我们还没创建任何分支呢。没错，不过没关系，因为我们还没有提交。先跑一个 `git status` 看看 Git 对我们这番手工活是否满意。

```bash
$ git status
On branch main
 
No commits yet
 
nothing to commit (create/copyfiles and use "git add"to track)
```

如果看到 “fatal: This operation must be run in a work tree”，大概率是因为你人在 `.git` 目录里。退回上一层就行。

好耶！接下来开始提交。

## 内容可寻址存储

在真正手工打造提交之前，我们得先研习一下古籍。

首先要搞清楚 Git 所说的“对象”是什么、有哪些类型，以及它是如何保存这些对象的。

Git 得保存的东西可不少。每次你创建提交，Git 要保存提交说明、有哪些文件、是谁提交的、时间戳，还有文件内容本身。

所有这些内容都会被存成“对象”。那它们长什么样？我们来挑一个已经很有分量的仓库瞧瞧。

```bash
$ git log -1
commit 84eae65b6780129486768e6497736c38bfdf9b3d (HEAD -> main, origin/main, origin/HEAD)
Author: Drew Silcock <redacted>
Date:   Mon Sep 30 21:39:55 2024 +0100
 
    Improve py 3.13 graphs and add extra section on scaling.
```

这是写这篇博客时仓库里的最新一次提交。你可以看到提交用它的 SHA-1 哈希标识： `84eae65b` （懒得把整串敲完了）。

接下来就开始显得聪明了——Git 保存提交、文件这些对象的方式 <sup><a href="https://www.appinn.com/drew-artisanal-git/#347b4e29-6c0c-4131-87a6-77e11ad27000">1</a></sup> ，不是把它们放在一个和内容脱离的文件名或键名底下（那样得在外面维护一份目录），而是直接基于内容本身。给对象做一遍 SHA-1，得到的就是你在磁盘上找到它的“钥匙”。

这种方式叫做 *Content Addressable Storage* （内容可寻址存储，简称 CAS），又叫固定内容存储。一旦认出这个模式，你会发现它无处不在。比如：

- Docker 镜像层
- InterPlanetary File System（IPFS）
- BitTorrent 的分布式哈希表（DHT）
- Nix

CAS 还有个副产品：如果文件重复，你并不需要存两份——它们哈希一样，自然会落到同一个位置。

### 提交对象（Commit objects）

要找到哈希 `84eae65b` 对应的提交，就得去 `.git/objects/84/eae65b6780129486768e6497736c38bfdf9b3d` 。对象是用 zlib 压缩的，我这里用 `pigz` 来解压：

```bash
$ cat.git/objects/84/eae65b6780129486768e6497736c38bfdf9b3d| pigz -d
commit 1136
tree 5a0be7720e65417e08034a64bc257bc56a60b4b3
parent e345662c7d53408eb2638cf0fdbae442fe6b68f
author Drew Silcock <redacted> 1727728795 +0100
committer Drew Silcock <redacted> 1727728795 +0100
gpgsig -----BEGIN PGP SIGNATURE-----
 
iQIzBAABCAAdFiEEaZwozZ5d++BpkqZmtEW8+mMmNyAFAmb7DJsACgkQtEW8+mMm
NyAZeBAAs2I1rodxTBpOnFUgNnl5Slf2o03VZlc7kvbw2miCUP5CkO40REHzGXXE
K3sJSUhObttTrKr0GjUChcvzBZBKoigawP+h3IeY07whhhTcnNaBXjQqzpcl+G5A
ryEVkQXdCqVRWAk3I/6Z3hFlfUogzbxihGoEKvjyMZtmfy0di0WAOJ+PLlTIEwJ
JSQYcUaA7l01ocIWy85MezGJHZEpurcBjzu5nkYCMGRw85u9tXXqjzaYh6Fu7WVE
HrHmBO8tEFF/WcQC1FonVggrOQOAsssuaMxwxKV/p4HRxP9lHGmzFCGfbKAY1bQ8
2dWgROwMAp6jtvLSX6OLu6i0O3+m6NAwTtKcOFDU+Jae4h2m1GC3/8qDukhK7o+e
5LJCLAZPtTvpai43COLRnF9iteV15H267WOxpIvXqbMBwIFcaaHepFMLA0Y39Kr3
6FHd1JAaaE6fiUe4rjNP5Wx6ZVLKdEYznjbxgxiRkr9dcemR5SUQtreHjaaLTo0E
9m6bEE1huZp+gu/dy9e7hgNORiwmkUP49r4/WPbNwwKrMxr5lD1ZwQk6DKEi6jAy
qBduJ4fdtamFlngnbJtoW0LHsdxROMwHkqs1Pz4zxpmeOZZEv0p0pzFhM30ta+Yj
QogBAyoRGHAZG2cze5uI8Cg7fr1A+uTqGmBAXexYN+/ok4+Bf/5g=
=gvSk
-----END PGP SIGNATURE-----
 
Improve py 3.13 graphs and add extra section on scaling.
```

如果把同样的内容再喂给 `hexyl` ，就能看到开头那个藏着的空字节：

```js
00000000: 63 6f 6d 6d 69 74 20 31 31 33 36 00 74 72 65 65  commit 1136.tree
00000010: 20 35 61 30 62 65 37 37 32 30 65 36 35 34 31 37   5a0be7720e65417
00000020: 65 30 38 30 33 34 61 36 34 62 63 32 35 37 62 63  e08034a64bc257bc
00000030: 35 36 61 36 30 62 34 62 33 0a 70 61 72 65 6e 74  56a60b4b3.parent
```

也就是说格式就是 `commit <内容长度>\x00<提交正文>` ，其它几种对象也都遵循这个套路。

### 树对象（Tree objects）

看到空字节后第一段信息是 `tree 5a0be7720e65417e08034a64bc257bc56a60b4b3` ，这告诉我们：哈希为 `5a0be772` 的“树”对象里列出了这次提交包含的文件。

```bash
$ cat.git/objects/5a/0be7720e65417e08034a64bc257bc56a60b4b3| pigz -d | hexyl
```

输出里能看到一堆文件名和 `100644` 这样的数字，明显是类 Unix 的文件权限。稍微好好排版一下，它其实是若干条重复的结构：

```js
(文件模式) (文件名)\x00(二进制哈希)(文件模式) (文件名)\x00(二进制哈希) …
```

文件模式是明文写的，所以我们能直接读到 `.gitignore` 的模式是 `100644` ，也就是 Greg Bacon 在 StackOverflow 上说的那种普通文件、权限 644。（顺便：我不知道 Git 在 Windows 上怎么处理这一套文件模式，如果你知道请留言！）

这一段数据还告诉我们 `.gitignore` 这个文件的哈希是 `d6 20 48 19 40 d7 4c de f6 ff ba be c0 d2 42 4f a7 b5 70 f6` 。由于 SHA-1 固定为 20 字节，我们完全不需要额外的分隔符，可以直接接着写下一条文件记录。

既然 Git 用这样的内容寻址方式，我们就能拿着这个哈希把文件本身找出来：

```bash
$ cat.git/objects/d6/20481940d74cdef6ffbabec0d2424fa7b570f6| pigz -d
blob 274
.vscode/

dist/
 

.astro/
 
...（后略）
```

你会先看到对象类型，这里是 “blob”，表示文件内容；接着是文件长度、一个空字节，然后才是文件本体（虽然这个例子一点都不惊险）。

Git 还准备了一些辅助命令，也就是它口中的 “plumbing” 命令，可以友好地打印对象内容： `git cat-file -p <哈希>` 会把对象内容打印出来， `-t` 会告诉你对象类型， `-s` 则打印大小。我们暂时没聊带注解的标签（它们同样是对象，和提交很像，所以在这里没那么关键）。轻量标签则仅仅是对提交哈希的引用，不是对象，因此存放在 `refs/` 目录里，和分支站在一起。

漂亮！

### 等等，那 diff 呢？

提交并不会保存文件的“差异”或“增量”，而是把之前与之后的完整内容都存下来。这其实是当年 Git 相比其它版本控制工具的一个巨大卖点，还没成为事实标准的时候就是如此 <sup><a href="https://www.appinn.com/drew-artisanal-git/#c5099c0b-3409-455b-a51f-fa0014a41dcc">2</a></sup> 。

当你运行 `git diff` 时，Git 会取出“之前”和“之后”的内容，然后调用一个差异比较器来展示结果。你甚至可以换成别的 diff 工具。我本人用的是 delta，听说 difftastic 和 diff-so-fancy 也很好用。

你也许会怀疑：这样仓库不会大得离谱吗？稍等，我们很快就聊到。

### 哈希前两位为什么变成目录？

好问题。某些文件系统不允许单个目录里放超过固定数量的文件，另外一些则必须顺序扫描整个目录来找文件——如果你是在往 Linux 内核这种有四百五十万个对象的仓库里提交，那就惨了。

SHA-1 的分布很均匀，理论上以 `00` 、 `1e` 、 `8f` 打头的对象数量差不多。Git 就利用这一点，随便挑一个子目录看看是不是有 27 个或更多文件 <sup><a href="https://www.appinn.com/drew-artisanal-git/#a279dc81-2a81-469e-b2e6-ef0736785102">3</a></sup> ，如果超过这个数，就说明 loose 状态的对象超过 6700 个了，于是就该把它们打包成 packfile，好省点空间。

### 打包时间到了

在 `.git/objects` 目录里你可能会看到几个非十六进制名字的子目录，一个叫 `info/` ，另一个叫 `pack/` 。

`info` 目录其实挺有意思，我也是为了写这篇博客才第一次见到——原来多个 Git 仓库是可以共享同一个对象库的，这样就能节省存储。老实说，我觉得这个目录只在非常小众的用例里会用到。

`pack` 目录则很常见，Git 会把打包好的对象放在这里。你可能要问 packfile 是啥？前面提过，当 Git 觉得 `.git/objects` 里的 loose 对象太多了（默认超过 6700 个），它就会把多个对象合成一个 packfile。这样可以在同一个文件里把多个对象一起压缩，压缩率更好。

但这会带来一个新问题：对象被塞进 packfile 之后，还怎么找到它？Git 支持针对每个 packfile 的索引文件，也支持最近加入的多 pack 索引（multi-pack index），可以用一个索引对应所有 packfile。如果你有多个 pack 索引，常见的小技巧是先查最近更新的那个索引，提高一点速度，但归根结底还是得把所有索引都翻一遍，才能定位到你的对象。

此时你可能会冒出两个疑问：

1. 这就能阻止仓库变得巨大吗？压缩好一些，但也不至于神奇到哪里去吧。
2. 如果 Git 保存的是文件的完整版本，那我克隆仓库时为什么会看到 “resolving deltas…”？

答案是：Git 确实会在 packfile 里存储“增量”。而且它会使用一些很奇怪的启发式算法，让打包后的对象尽可能高效——既包括增量构造，也包括后续的压缩过程。

尤其是，Git 并不会把时间上最早的版本当作“基准”，然后按顺序叠加增量。它甚至不在乎两个对象是不是同一个文件——Git 会找那些内容相似的对象，选出哪个更适合拿来当增量的基准，而不管这些对象是不是来自同一份文件。

正是靠着这套聪明但略带魔法味儿的启发式打包策略，Git 才让仓库的体积保持在可控范围内。还有一个帮手是 Git 会把“不可达”的对象丢掉，让仓库只留下你真正需要的东西。

### 倒垃圾时间

值得再聊聊 Git 的垃圾回收，也就是清理那些“不可达”对象——这门知识关键时刻能救你一命。

万一你不小心删掉了一个装着重要提交的分支，可能会哀嚎：“天呐，我的宝贝文件全没了！”别慌：Git 在你删除分支的时候并不会立刻抹掉那些提交和文件。

Git 会把你对每个分支以及 `HEAD` 做过的操作都记在 `.git/logs/` 目录里。针对 `HEAD` 的操作写在 `.git/logs/HEAD` ，而像分支这样的引用会对应 `.git/logs/refs/heads/main` 这样的文件。每条日志就是一次“动作”：提交、从远端拉取、合并分支、切换分支（切换只会出现在 `HEAD` 的日志里，不会出现在具体分支的日志里）等等。

因为这些日志记录的是针对“引用”的操作，所以叫 reference log，简称 reflog。

默认情况下，reflog 会保留 90 天（可以通过 `gc.reflogExpire` 配置）。只要提交还在 reflog 中，Git 就认为它依旧是“可达”的。即便某个对象真的从 reflog 里消失了，Git 还会额外给它两周缓冲期（ `gc.pruneExpire` ），之后才会做垃圾回收。这意味着你通常还能靠这里提到的手工方法，或是运行 `git reflog` 再配合 `git checkout` / `git reset --hard` 把工作救回来。

想进一步了解 reflog 等内容，推荐去读 Julia Evans 的小册子《Oh Shit, Git!》。

不过得提醒一句：reflog 只存在于你本地的仓库里。如果你删掉 `.git` 目录或者重新克隆了一个干净的仓库，那些日志就没了，删掉的分支自然也救不回来。

## 创建我们的第一个提交

既然已经搞懂 Git 对象，是时候亲自造一个提交了。

```bash
$ echo-e "Has spring come indeed?\nOn that nameless mountain lie\nThin layers of mist.\n\n  - Matsuo Bashō"> haiku.txt
$ git status
On branch main
 
No commits yet
 
Untracked files:
  (use "git add <file>..."to include inwhat will be committed)
        haiku.txt
 
nothing added to commit but untracked files present (use "git add"to track)
```

照规矩我们应该先把文件添加到暂存区再提交，不过咱们就是要手工艺路线。再说索引文件 `.git/index` 是个二进制文件：一方面没什么好看的，另一方面也不太容易只靠命令行去鼓捣它。

如果你对 index 文件的格式有浓厚兴趣，喜欢端着一杯热摩卡阅读描述二进制格式的内部文档，可以去翻翻 Git 文档里关于 index 格式的参考页面： [https://git-scm.com/docs/index-format](https://git-scm.com/docs/index-format) <sup><a href="https://www.appinn.com/drew-artisanal-git/#c852f2ee-426b-4dc5-a62b-cbe0a1899e16">4</a></sup> 。

### 构造文件 blob

我们要从零拼出一个提交。先做 blob（文件内容），再用它组装出树对象，最后才能从树里生成提交对象。

```bash
$ cathaiku.txt | wc-c
      94
 

$ echo-e "blob 94\x00$(cat haiku.txt)"| sha1sum
e5d59773e77daf9f9b9129781ca77d475a451831  -
 
$ mkdir-p .git/objects/e5
$ echo-e "blob 94\x00$(cat haiku.txt)"| pigz -z > .git/objects/e5/d59773e77daf9f9b9129781ca77d475a451831
 

$ git cat-file-p e5d59773e77daf9f9b9129781ca77d475a451831
Has spring come indeed?
On that nameless mountain lie
Thin layers of mist.
 
  - Matsuo Bashō
```

看起来不错！

### 构造树对象

接下来用刚才的 blob 做一个只有这份文件的树对象，文件模式还是 `100644` 。

```bash
$ functionhex-to-bytes() { printf"$(printf "$1" | sed 's/../\\x&/g')"; }
```

Fish 用户的话，函数得写成这样：

```bash
functionhex-to-bytes; printf"$(printf "$argv[1]" | sed 's/../\\\\x&/g')"; end
```

继续创建树对象：

```bash
$ printf"100644 haiku.txt\x00$(hex-to-bytes e5d59773e77daf9f9b9129781ca77d475a451831)"| hexdump -C
00000000  31 30 30 36 34 34 20 68 61 69 6b 75 2e 74 78 74  |100644 haiku.txt|
00000010  00 e5 d5 97 73 e7 7d af 9f 9b 91 29 78 1c a7 7d  |....s.}....)x..}|
00000020  47 5a 45 18 31 0a                                |GZE.1.|
 
$ printf"100644 haiku.txt\x00$(hex-to-bytes e5d59773e77daf9f9b9129781ca77d475a451831)"| wc-c
      37
 
$ printf"tree 37\x00100644 haiku.txt\x00$(hex-to-bytes e5d59773e77daf9f9b9129781ca77d475a451831)"| sha1sum
4aff48f6390a65b88d343ea5d23c03007646b5c2  -
 
$ mkdir-p .git/objects/4a
$ printf"tree 37\x00100644 haiku.txt\x00$(hex-to-bytes e5d59773e77daf9f9b9129781ca77d475a451831)"| pigz -z > .git/objects/4a/ff48f6390a65b88d343ea5d23c03007646b5c2
```

### 构造提交对象

树有了，总算可以开工造提交了！

```bash
$ date+'%s %z'
1752659127 +0100
 

$ cat<<'EOF'> /tmp/my-commit
tree 4aff48f6390a65b88d343ea5d23c03007646b5c2
author Drew Silcock <redacted> 1752659127 +0100
committer Drew Silcock <redacted> 1752659127 +0100
 
Initial artisanal commit.
EOF
```
```bash
$ cat/tmp/my-commit| gpg --armor --detach-sign
-----BEGIN PGP SIGNATURE-----
 
iQIzBAABCAAdFiEEaZwozZ5d++BpkqZmtEW8+mMmNyAFAmh3diEACgkQtEW8+mMm
NyDomRAAvWYhK9Eg+ZjmChFR2ZX9bB/KZH+H3ksziy2UHp8LiaHgOb3Ira02rpSm
LvVQjxmgurzYBd3nl1e/8E+V3TH1kGOzmvaoCcjJkSUj6togvD7+eImulc1/xkri
q/qqPXxvj2UoRMbSc4cVy/8SZ/MTxNWtCJsuFRe6iKLRiqk67h3PY+gvebCuJteC
TevKxWV/ra+NRX2Q0w52SEUpGTVcnnxYPyMEi28Kmd9VZUsOvuC43RMm/p7u/eiC
kAzJ3GKN4oQvN/3Xz8akb09VX66M/xbMYNv/J0pbSdeIGofMDfLA3oKeZzhrvUVf
zsrpiJ9kq2CTGIuZMJZQvPc8aEEMbr/PAHgSnSTicayon7JLoi5aaoyhZLCD+pgK
Sd6OMMjrKs61UL2qxelVVde2tZumfOL4GmILrhxQgqZbZsdfDUvPMee9yFkEZVam
re8ekkUlYmlmckTqJ0yQ7VTLYhdxPN+0DRynuiKKQaQlsCHhQi2MTYEn9l+mfbrO
7gUAe697+kDwo2VECs4Z7wtPG9F+kGNFpsC0CnGMWjKRR8ZBV9BBiLyc/SZgNd9Q
MZLQWPPvnMw0YlS59rHbUk3VwebxBKx8vX2WBt1NPFtmzkFRr73yL+e69JczspoM
t6FRuXH0bTtF+uVf7qD0saFXCC9lphLYFe5PuyzpIKwWbazGDFA=
=rE6v
-----END PGP SIGNATURE-----
```
```bash
$ cat<<'EOF'> /tmp/my-commit-signed
tree 4aff48f6390a65b88d343ea5d23c03007646b5c2
author Drew Silcock <redacted> 1752659127 +0100
committer Drew Silcock <redacted> 1752659127 +0100
gpgsig -----BEGIN PGP SIGNATURE-----
 
 iQIzBAABCAAdFiEEaZwozZ5d++BpkqZmtEW8+mMmNyAFAmh3diEACgkQtEW8+mMm
 NyDomRAAvWYhK9Eg+ZjmChFR2ZX9bB/KZH+H3ksziy2UHp8LiaHgOb3Ira02rpSm
 LvVQjxmgurzYBd3nl1e/8E+V3TH1kGOzmvaoCcjJkSUj6togvD7+eImulc1/xkri
 q/qqPXxvj2UoRMbSc4cVy/8SZ/MTxNWtCJsuFRe6iKLRiqk67h3PY+gvebCuJteC
 TevKxWV/ra+NRX2Q0w52SEUpGTVcnnxYPyMEi28Kmd9VZUsOvuC43RMm/p7u/eiC
 kAzJ3GKN4oQvN/3Xz8akb09VX66M/xbMYNv/J0pbSdeIGofMDfLA3oKeZzhrvUVf
 zsrpiJ9kq2CTGIuZMJZQvPc8aEEMbr/PAHgSnSTicayon7JLoi5aaoyhZLCD+pgK
 Sd6OMMjrKs61UL2qxelVVde2tZumfOL4GmILrhxQgqZbZsdfDUvPMee9yFkEZVam
 re8ekkUlYmlmckTqJ0yQ7VTLYhdxPN+0DRynuiKKQaQlsCHhQi2MTYEn9l+mfbrO
 7gUAe697+kDwo2VECs4Z7wtPG9F+kGNFpsC0CnGMWjKRR8ZBV9BBiLyc/SZgNd9Q
 MZLQWPPvnMw0YlS59rHbUk3VwebxBKx8vX2WBt1NPFtmzkFRr73yL+e69JczspoM
 t6FRuXH0bTtF+uVf7qD0saFXCC9lphLYFe5PuyzpIKwWbazGDFA=
 =rE6v
 -----END PGP SIGNATURE-----
 
Initial artisanal commit.
EOF
```
```bash
$ cat/tmp/my-commit-signed| wc-c
    1027
 
$ echo-e "commit 1027\x00$(cat /tmp/my-commit-signed)"| sha1sum
d62016426c1b7b4125d47bad267aeaaa78bb817c  -
 
$ mkdir-p .git/objects/d6
$ echo-e "commit 1027\x00$(cat /tmp/my-commit-signed)"| pigz -z > .git/objects/d6/2016426c1b7b4125d47bad267aeaaa78bb817c
```

### 引用到底是什么？

对象就是有内容的东西——它有类型、有大小，存放在 `.git/objects` 里，要么是松散文件，要么被打进 packfile。引用则只是一个哈希，没有内容本体。它们都住在 `.git/refs` 里，常见的有三种：

- 本地分支：位于 `.git/refs/heads/` ，例如 `.git/refs/heads/main` 代表本地的 `main`
- 远端分支：位于 `.git/refs/remotes/` ，例如 `.git/refs/remotes/origin/main` 代表远端 `origin` 上的 `main`
- 轻量标签：位于 `.git/refs/tags/` ，比如 `.git/refs/tags/v1.2.3` 。前面提过，带注解的标签因为包含信息，本身是对象。

当你执行 `git fetch` 时，Git 会查看服务器上的引用是否和你放在 `.git/refs/remotes` 里的那些匹配，然后据此更新本地。Git 告诉你 “Your branch is up to date with ‘origin/main’.”，意思就是本地分支指向的哈希和远端分支一样，也就是 `.git/refs/heads/main` 与 `.git/refs/remotes/origin/main` 完全相同。

引用文件里到底有什么？答案只有一个提交哈希！我们已经知道自己的提交哈希了，来手工创建 `main` 分支，让它指向刚刚那份匠心提交：

```bash
$ echo"d62016426c1b7b4125d47bad267aeaaa78bb817c"> .git/refs/heads/main
```

### 检查成果

现在就能用熟悉的“瓷器”命令看看我们的成果：

```bash
$ git status
On branch main
nothing to commit, working tree clean
 
$ git branch
* main
 
$ git log --show-signature
commit d62016426c1b7b4125d47bad267aeaaa78bb817c (HEAD -> main)
gpg: Signature made Wed 16 Jul 10:51:29 2025 BST
gpg:                using RSA key 699C28CD9E5DFBE06992A666B445BCFA63263720
gpg: Good signature from "Drew Silcock <redacted>"[unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 699C 28CD 9E5D FBE0 6992  A666 B445 BCFA 6326 3720
Author: Drew Silcock <redacted>
Date:   Wed Jul 16 10:45:27 2025 +0100
 
    Initial artisanal commit.
```

搞定啦！

## 故障排查

如果过程中卡壳，或者遇到 `error: bad tree object HEAD` 这样的报错，试试运行 `git fsck --full` ，通常它会告诉你是哪一步出了岔子。

## 后续话题

写到这里已经够久了，但还有不少有趣的话题没展开——欢迎留言、发邮件或者对着风大喊，告诉我你更想在后续文章里看到哪一个：

- 储藏（stash）——它到底怎么回事？（剧透：本质上也就是提交）
- Reflog ——我想多了解一点这项隐藏技能，看看怎样把心爱的文件从坟墓里救回来。
- Packfile 的格式与索引 ——想继续听听 packfile 长啥样，以及 Git 是怎么在里面翻找的。
- Index 文件格式 ——你居然略过了这个！请立刻写一整篇博客把这份二进制格式按位讲清楚，否则我要提起诉讼。
- 网络通信 ——Git 是怎么和服务器对话的？ `https://github.com/...`、 `[email protected]:...`、 `[email protected]:...` 在传输层面到底有什么区别？老实说我也不懂，但听起来很好玩。

## 总结

研究这些内部细节挺好玩，但千万别在真仓库里这样操作——你会把东西搞坏，然后心情大受打击。

希望你觉得这些内容有意思。说实话，如果你能坚持读到这里，要么是直接跳到结尾看看有没有什么劲爆内容（抱歉让你失望了），要么就是那种会把手工艺 Git 仓库技术长文从头读到尾的人。不管哪一种，希望你读得开心，也顺便学到点啥。

如果要我只留下一个 takeaway，那就是 Git 设计的优雅。了解底层文件格式之后你会发现，它其实没有那么复杂。没错，Git 里还有 rebase、reflog 等等复杂功能，但 Git 并不是魔法，你完全可以用自己喜欢的语言从零实现一个 git clone（双关奉上）。只要暂时忽略那些更复杂的东西，这事并没有想象中难。

## 更新

- 2025-07-17：补充了关于 packfile、垃圾回收和 reflog 的细节。

## 延伸阅读

这次写作的主要灵感来自 Julia Evans 的 Git 博文。如果你觉得本文有意思，不妨去看看她写的 Git 文章。看完这些，再把其他的也都读一遍，真的都很棒。

- Julia Evans – Inside.git – [https://jvns.ca/blog/2024/01/26/inside-git](https://jvns.ca/blog/2024/01/26/inside-git)
- Julia Evans – In a git repository, where do your files live? – [https://jvns.ca/blog/2023/09/14/in-a-git-repository–where-do-your-files-live-/](https://jvns.ca/blog/2023/09/14/in-a-git-repository--where-do-your-files-live-/)
- Git Reference Documentation – The Git index file has the following format – [https://git-scm.com/docs/index-format](https://git-scm.com/docs/index-format)
- Unpacking Git packfiles – [https://codewords.recurse.com/issues/three/unpacking-git-packfiles](https://codewords.recurse.com/issues/three/unpacking-git-packfiles)
- Abin Simon (@meain) – What is in that.git directory? – [https://blog.meain.io/2023/what-is-in-dot-git/](https://blog.meain.io/2023/what-is-in-dot-git/)
- Dulwich Project Documentation – Git File format – [https://www.samba.org/~jelmer/dulwich/docs/tutorial/file-format.html](https://www.samba.org/~jelmer/dulwich/docs/tutorial/file-format.html)

## 脚注

1. 不过并不是所有东西都这样存。 [↩︎](https://www.appinn.com/drew-artisanal-git/#347b4e29-6c0c-4131-87a6-77e11ad27000-link)
2. 当然我知道有人依旧用 Perforce、Mercurial 之类的工具。至少我离开游戏行业的那会儿，Perforce 还挺受欢迎。 [↩︎](https://www.appinn.com/drew-artisanal-git/#c5099c0b-3409-455b-a51f-fa0014a41dcc-link)
3. 由于 SHA-1 分布足够均匀，Git 只需盯着一个目录看里面有没有 27 个以上的文件就行。 [Junio Hamano 选了 `17/`](https://github.com/git/git/commit/2c3c4399477533329579ca6b84824ef0b125914f#diff-198fce92f2df7eeb56494f2e86aa173543973ad6f196baccf869ffc5fb19b770R83) 。别问我为什么，可能那是他最爱的十六进制数？ [↩︎](https://www.appinn.com/drew-artisanal-git/#a279dc81-2a81-469e-b2e6-ef0736785102-link)
4. 说实话，我就是那种人。 [↩︎](https://www.appinn.com/drew-artisanal-git/#c852f2ee-426b-4dc5-a62b-cbe0a1899e16-link)