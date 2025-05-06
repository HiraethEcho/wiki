---
title: 各种同步方法
toc: true
tags:
    - tools
date: 2024-12-21
---

# 各种云同步方法

## 云盘

- koofr
- onedrive
- ali

## 文件同步方法

- webdav 协议
- rclone 命令行工具

rclone --filter-from list.txt localdir remote:target

without
```txt
- **.git/**
```
not ignore `.git` in localdir 

without
```txt
- *.git
```
not ignore `.git` in subdir in localdir 

have absolute no idea why

```txt
- *.git
- **.git/**
```
can ignore all `.git` folders

## 管理

alist
