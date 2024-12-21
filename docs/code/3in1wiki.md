---
title: 用mkdocs、obsidian、zotero搭建个人wiki
toc: true
tags:
  - second_brain
published: false
date: 2024-10-15 13:48:01
---
# 使用mkdocs、obsidian、zotero搭建个人在线wiki
工作流是，在zotero中阅读和标记。用zotero-better-notes插件把标记变成文本格式，并且导出到obsidian的库文件中，进一步编辑（或者用nvim编辑）。在obsidian中更方便笔记链接跳转，zotero只是文献管理和阅读。然后把笔记同步到mkdocs的git仓库（实际上是把mkdocs的文件夹软链接到obsidian库），用github pages搭建在线wiki。

主要需要注意的，是文件名和链接格式，即`[markdown格式的链接](这里应该写什么路径)`，使得zotero、obsidian、在线网页的跳转都正常。实际上有两个跳转，一个是到md文件的跳转，另一个是到pdf的跳转。前者是obsidian或网页内的跳转，相对简单；后者是跳转到zotero中的pdf，这个实际上只有我能用，因为涉及到zotero的数据库。

## Zotero
关键是`zotero-better-note`插件的模板设置：

从注释创建笔记；讲笔记插入到其他笔记；将笔记导出。

> [!important]
> 导出路径和模板中链接路径有硬编码部分，需要根据具体情况手动修改。

## obsidian
需要在设置中选择使用`markdown`风格链接而不是`wiki`风格。根目录`/`是obsidian的库文件夹。

根据mkdocs的在github的库名称，要在obsidian的库文件夹内有一个同名子文件夹。实际上可以软链接到git仓库。

## mkdocs
基本上就是直接使用模板。建议使用二级域名的github pages，即创建仓库（例如`wiki`），那么在线网页会是`https://<username>.github.io/wiki`，然后
```sh
ln -s /path/to/git/repo/docs /path/to/obsidian/repo/wiki
```
这样所有路径都是匹配的。
## 总结

> [!note]
> 似乎需要一个markdown和latex转换的脚本。其中markdown应该混写html比较好。似乎lua就可以，那么就可以做nvim插件。
