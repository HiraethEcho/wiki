---
title: caldav链接icloud
toc: true
tags:
date: 2025-06-30
---

# 学习笔记

## caldav

ref:

- https://cyp0633.icu/post/icloud-calendar-sync/
- https://www.aurinko.io/blog/caldav-apple-calendar-integration/

云上贵州服务器

```
https://<server-id>-caldav.icloud.com.cn/<user-id>/calendars/<calendar-id>/
```

在网页版icloud的日历界面打开开发者工具，然后找`Network`里的`Get`请求，过滤`XHR`，然后找类似 `https://<server-id>-calendarws.icloud.com.cn/ca/eventdetail/<calendar-id>/<event-id>?clientBuildNumber=<some-random-text>&clientId=<some-random-text>&clientMasteringNumber=<some-random-text>&clientVersion=<some-random-text>&dsid=<user-id>&lang=<some-random-text>&requestID=<some-random-text>&usertz=<some-random-text>`的内容。

## vdirsync

用 [vdirsync](https://vdirsyncer.readthedocs.io/) 将`icloud`的日历同步到本地，以`.ics`格式存储。

```conf
[storage cal]
type = "caldav"
url = "https://caldav.icloud.com/"
username = "..."
password = "..."

[storage card]
type = "carddav"
url = "https://contacts.icloud.com/"
username = "..."
password = "..."
```

这里`username`就是用户名，而`password`是双重认证后再生成的app密码。服务器的话中国应该要`url = "https://caldav.icloud.com.cn/"`

但是有两个问题：

- 不支持`VTODO`格式，apple的提醒事项似乎不是这个协议。
- `icloud`如果有多个日历的话，标识id很长，难以记忆或使用，例如`87BBBA05-934B-4D5A-BB27-771A9CD413A2`。或者其中一个会被记为`home`，但是不知道什么规则。例如同步到`.local/share/calendars`后
    ```
    ~/.local/share/calendars
    ├── 87BBBA05-934B-4D5A-BB27-771A9CD413A2
    └── home
    ```
