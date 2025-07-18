---
title: "Hugo 设置不同页面共用一套 Twikoo 评论"
source: "https://blog.guole.fun/posts/hugo-gongyong-twikoo/"
author:
  - "[[八章,guole.fun@qq.com]]"
date: "2025-05-06T21:08:40+08:00"
description: "为你的网站启用简洁，美观，友好且免费，同时功能强大的 Twikoo 评论系统！Twikoo，yyds"
dg-publish: true
---
[Guo Le's Blog](https://blog.guole.fun/posts/hugo-gongyong-twikoo/)

100%

原创

## Hugo 设置不同页面共用一套 Twikoo 评论

深圳 | 617 | 2分钟

上周给 blog.guole.fun 接入了 Twikoo 评论系统，这次在两个页面共用同一个留言板（Twikoo）内容。

本站的留言板有个独立的`.md` 文件，左上角 icon 点击会翻转展示个人信息。我希望这里展示的留言版，和菜单栏留言板是同一套内容。那怎么做呢？

好在发现了 Twikoo 支持 `path` 参数，折腾半天也没整上，Twikoo 作者，虹墨在群里指导了下， [并且立即发了个简短教程](https://www.imaegoo.com/2021/twikoo-path/ ) ，不胜感激。考虑到 Hugo 有些特别的地方，自己也研究了下才跑通，因此记录下来分享。

### 配置 Twikoo

首先搞个独立的模板文件 `comments.html` ，放在了 `/layouts/partials/` 目录，因为其他文章需要各自的 Twikoo，所以搞个特别的，专门在留言板页面引用。

```html
html <script>window.TWIKOO_MAGIC_PATH="留言板"</script>

    <div class="liuyan" >  <!--  自己加了个标题  --> 

        <p>留言板</p>

      </div>

    <div id="tcomment"></div>

      <script src="https://jsd.guole.fun/npm/twikoo@1.3.1/dist/twikoo.all.min.js"></script>

      <script>

      twikoo.init({

        envId: '*******',// 你的环境id

        el: '#tcomment',

        // region: 'ap-guangzhou', // 环境地域，默认为 ap-shanghai，如果您的环境地域不是上海，需传此参数

        path: 'window.TWIKOO_MAGIC_PATH||window.location.pathname', // 用于区分不同文章的自定义 js 路径，如果您的文章路径不是 location.pathname，需传此参数

      })

</script>
```

重点是这个 `path` 设置为 `window.TWIKOO_MAGIC_PATH||window.location.pathname` ，同时在需要共用的页面加一个 `<script>window.TWIKOO_MAGIC_PATH="留言板"</script>` ，因此我这个模板文件里也加了。注意这里的 `留言板` 名称要两处保持一致。

### 多个模板页面引用

我使用的是 Hugo-Dream 主题，因此还需要在 `nave.html` 模板中，也加一句 `window.TWIKOO_MAGIC_PATH||window.location.pathname` 。大功告成！

```js
jswindow.TWIKOO_MAGIC_PATH||window.location.pathname
```

创建一个菜单栏入口进入的留言板页面（与上面模板名称保持一致） `hugo new comments.md` 。 [戳这里查看我的留言板](https://blog.guole.fun/message/ "戳这里查看我的留言板") ，再点击左上角第一个 icon 瞧瞧！

现在，两个页面 Twikoo 内容一致啦~~

1. [Hugo 国内腾讯云域名注册](https://blog.guole.fun/posts/blog-yuming/ "Hugo 国内腾讯云域名注册")
2. [使用 Hugo + Dream主题搭建个人Blog](https://blog.guole.fun/posts/hugo-blog/ "Hugo搭建个人Blog")
3. [Hugo 启用 Twikoo 评论系统](https://blog.guole.fun/posts/hugo-twikoo/ "Hugo 启用 Twikoo 评论系统")
4. [Hugo 设置不同页面共用一套 Twikoo 评论](https://blog.guole.fun/posts/hugo-gongyong-twikoo/ "Hugo 设置不同页面共用一套 Twikoo 评论")
5. [Hugo 实现 Coding + Github 自动化部署](https://blog.guole.fun/posts/hugo-coding-github/ "Hugo 实现 Coding + Github 自动化部署")


[https://blog.guole.fun/posts/hugo-gongyong-twikoo/](https://blog.guole.fun/posts/hugo-gongyong-twikoo/)

本站所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。转载请注明来自 [Guo Le's Blog](https://blog.guole.fun/) ！
