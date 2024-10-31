---
title: 用mkdocs、obsidian、zotero搭建个人wiki
toc: true
tags: []
published: false
date: 2024-10-15 13:48:01
---
# 使用mkdocs、obsidian、zotero搭建个人在线wiki
工作流是，在zotero中阅读和标记。用zotero-better-notes插件把标记变成文本格式，并且导出到obsidian的库文件中，进一步编辑（或者用nvim编辑）。在obsidian中更方便笔记链接跳转，zotero只是文献管理和阅读。然后把笔记同步到mkdocs的git仓库（实际上是把mkdocs的文件夹软链接到obsidian库），用github pages搭建在线wiki。

主要需要注意的，是文件名和链接格式，即`[markdown格式的链接](这里应该写什么路径)`，使得zotero、obsidian、在线网页的跳转都正常。实际上有两个跳转，一个是到md文件的跳转，另一个是到pdf的跳转。前者是obsidian或网页内的跳转，相对简单；后者是跳转到zotero中的pdf，这个实际上只有我能用，因为涉及到zotero的数据库。