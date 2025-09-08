---
title: 多个博客网站管理
tags:
    - github
    - site
date: 2025-08-20
status: draft
---

# 多个博客网站管理

在[用obsidian统一管理博客内容](/wiki/dev/blogs)后，考虑绑定域名，以及重新整理管理方式。

## 问题及分析

### 前提

框架：

- hexo
- hugo
- jekyll
- mkdocs

内容仓库

- 每个框架一个仓库，包含内容和配置
- 所有内容在一个仓库，且只有内容；不同框架配置在各自仓库

托管位置

- github pages
- cloudflare pages
- netlify, vercel, etc

绑定的域名

- 在一个域名下
    - (blog.)example.com/hexo
    - (blog.)example.com/hugo
    - ···
- 在不同子域名下
    - hugo.example.com
    - hexo.example.com
    - ···

### 分析

### 总结

## 操作

## 参考 deepseek

**假设我有一个域名example.com和cloudflare账号。有多个静态博客github仓库，分别使用hugo,hexo,mkdocs,jekyll等。我想要把它们和域名绑定，怎么安排他们的路径，以及重定向的方法**

例如：

- `example.com/tech/` -> 指向 Hugo 博客
- `example.com/life/` -> 指向 Hexo 博客
- `example.com/docs/` -> 指向 MkDocs 站点
- `example.com/notes/` -> 指向 Jekyll 博客

### 方案一：使用 Cloudflare Workers（推荐，最灵活）

1.  **为每个博客配置原始地址**：
    - 确保你的每个博客都已经部署并可以通过一个独立的 URL 访问（例如 `hugo-blog.pages.dev`, `hexo-site.netlify.app`, `mkdocs-docs.vercel.app` 等）。记下这些 URL。

2.  **在 Cloudflare 中创建 Worker**：
    - 登录 Cloudflare 仪表板，进入 **Workers & Pages**。
    - 点击 **Create application** -> **Create Worker**。
    - 给你的 Worker 起个名字，比如 `blog-router`。

3.  **编写 Worker 路由脚本**：
    将下面的代码替换到 Worker 编辑器中，并根据你的实际情况修改 `pathMappings` 对象。

    ```javascript
    // 路径到上游（源站）URL 的映射
    const pathMappings = [
        { path: "/tech", upstream: "https://hugo-blog.pages.dev" },
        { path: "/life", upstream: "https://hexo-site.netlify.app" },
        { path: "/docs", upstream: "https://mkdocs-docs.vercel.app" },
        { path: "/notes", upstream: "https://jekyll-blog.github.io" },
        // 可以继续添加更多映射...
    ];

    export default {
        async fetch(request, env, ctx) {
            const url = new URL(request.url);
            const pathname = url.pathname;

            // 遍历映射，找到匹配的路径
            for (const mapping of pathMappings) {
                if (
                    pathname.startsWith(mapping.path + "/") ||
                    pathname === mapping.path
                ) {
                    // 构建新的上游URL
                    const upstreamUrl = new URL(mapping.upstream);
                    // 将请求的路径替换到上游URL上
                    upstreamUrl.pathname =
                        pathname.replace(mapping.path, "") || "/";
                    upstreamUrl.search = url.search;

                    // 创建新的请求对象，保留原始方法和头部
                    const newRequest = new Request(upstreamUrl, {
                        method: request.method,
                        headers: request.headers,
                        body: request.body,
                        redirect: "manual", // 让Worker处理重定向
                    });

                    // 向上游发送请求并获取响应
                    let response = await fetch(newRequest);

                    // 可选：修复响应头（如Location头中的重定向）
                    if (response.headers.get("location")) {
                        let location = response.headers.get("location");
                        // 如果上游返回的重定向指向它自己的域名，将其替换为我们的域名和路径
                        if (location.startsWith(mapping.upstream)) {
                            location = location.replace(
                                mapping.upstream,
                                "https://example.com" + mapping.path,
                            );
                            response = new Response(response.body, response);
                            response.headers.set("location", location);
                        }
                    }

                    return response;
                }
            }

            // 如果没有匹配的路径，可以返回404，或者重定向到一个默认页面
            return new Response("Blog not found", { status: 404 });
        },
    };
    ```

4.  **配置 Worker 路由**：
    - 在 Worker 详情页，点击 **Triggers** -> **Add Route**。
    - 添加路由模式：`example.com/tech/*` 和 `example.com/life/*` 等。你也可以直接添加 `example.com/*` 来让这个 Worker 处理所有子路径。

**完成！** 现在访问 `example.com/tech/about/`，Worker 会代理请求到 `hugo-blog.pages.dev/about/`，并将内容返回给用户，用户的浏览器地址栏始终显示的是 `example.com`。

### 方案二：使用 Cloudflare Pages（简单，但有限制）

如果你的博客都部署在支持**自定义路径**的平台上（如 Netlify, Vercel），或者你愿意使用 Cloudflare Pages 来托管其中一个，可以使用这个方法。

**步骤**：

1.  **在 Cloudflare Pages 中创建一个项目**（例如，用来托管你的 Hugo 博客）。
2.  在构建设置中，**不要**使用根路径 (`/`)，而是使用你想要的子路径，例如 `/tech`。
    - 对于 Hugo：设置 `baseURL = "https://example.com/tech"`
    - 对于 Hexo：在 `_config.yml` 中设置 `root: /life/`
    - 对于 MkDocs：在 `mkdocs.yml` 中设置 `site_url: https://example.com/docs`
    - **关键**：你必须在你各个博客的配置文件中提前设置好 `baseURL` 或 `root` 为 `https://example.com/你的路径`，这样它们生成的所有内部链接（CSS, JS, 文章链接）才会是正确的。
3.  为这个 Pages 项目设置自定义域名 `example.com`。
4.  对于其他博客，在 Cloudflare DNS 中为 `example.com` 设置 `CNAME` 记录指向它们的托管地址（如 `hexo-site.netlify.com`），但**这通常会导致冲突**，因为一个根域名通常只能指向一个地方。

---

**我的博客资源在github多个repo中呢**

### 核心思路：分离“构建托管”和“路由代理”

1.  **构建与托管 (Build & Host)**：为每个 GitHub repo 选择一个平台来自动构建代码并生成静态网站，获得一个独立的在线地址（如 `my-hugo-site.pages.dev`）。
2.  **路由与代理 (Route & Proxy)**：使用 Cloudflare Workers（推荐方案）根据路径（如 `/tech`），将用户请求无缝代理到对应的托管地址。

步骤一：为每个 GitHub Repo 选择托管平台（构建与托管）

#### 选项 1: Cloudflare Pages（首选，与 Workers 同平台）

1.  在 Cloudflare 仪表板中，进入 **Workers & Pages**。
2.  点击 **Create application** -> **Pages** -> **Connect to Git**。
3.  选择你的 GitHub 账号，授权并选择你的一个博客仓库（例如 `hugo-blog`）。
4.  **配置构建设置**：
    - **框架预设**：Cloudflare Pages 能自动检测 Hugo, Hexo, Jekyll 等，并给出推荐配置。通常无需修改。
    - **构建命令**：例如 Hugo 是 `hugo`，Hexo 是 `hexo generate`，MkDocs 是 `mkdocs build`。
    - **构建输出目录**：`public` (Hugo, Hexo), `site` (MkDocs), `_site` (Jekyll)。
5.  点击 **Save and Deploy**。成功后，你会获得一个免费的 `.pages.dev` 子域名，如 `your-hugo-blog.pages.dev`。**记下这个地址**。
6.  **重复此过程**，为你的 Hexo、MkDocs、Jekyll 等每个仓库都创建一个 Cloudflare Pages 项目。

#### 选项 2: Netlify / Vercel（优秀的替代方案）

1.  登录 Netlify 或 Vercel。
2.  连接你的 GitHub 账号，导入一个仓库。
3.  配置构建命令和输出目录。
4.  部署后，你会获得一个 `.netlify.app` 或 `.vercel.app` 的免费子域名。**记下这个地址**。

#### 选项 3: GitHub Pages（原生但稍弱）

**如何设置**：
在仓库的 `Settings` -> `Pages` 中，选择构建源（通常是 `GitHub Actions` 或某个分支）。成功后，地址通常是 `https://<你的github用户名>.github.io/<仓库名>`。

### 步骤二：配置每个博客的 baseURL（关键！）

这是**最重要的一步**，否则你的博客会出现 CSS/JS 加载失败、链接错误等问题。

> [!important] 路径设置
> 你必须在每个博客的**源码配置文件**中，将 `baseURL` 设置为你的**最终目标路径**，即 `https://example.com/你的路径/`。

- **Hugo** (`config.toml`):
    ```toml
    baseURL = "https://example.com/tech/"
    ```
- **Hexo** (`_config.yml`):
    ```yml
    url: https://example.com/life/
    root: /life/
    ```
- **MkDocs** (`mkdocs.yml`):
    ```yml
    site_url: https://example.com/docs/
    ```
- **Jekyll** (`_config.yml`):
    ```yml
    url: "https://example.com"
    baseurl: "/notes" # 注意这里没有末尾的斜杠
    ```

**为什么这么做？** 这样博客在构建时，所有资源的链接路径都会变成 `/tech/css/style.css` 而不是 `/css/style.css`。当用户通过 `example.com/tech/` 访问时，所有资源才能被正确加载。

### 步骤三：使用 Cloudflare Workers 进行路由（代理）

现在你已经有了一组托管地址（我们称之为“上游地址”）。假设：

- Hugo 博客：`https://hugo-blog.pages.dev` (对应 `baseURL: /tech/`)
- Hexo 博客：`https://hexo-site.pages.dev` (对应 `baseURL: /life/`)
- MkDocs 站点：`https://mkdocs-docs.pages.dev` (对应 `baseURL: /docs/`)
- Jekyll 博客：`https://jekyll-notes.pages.dev` (对应 `baseURL: /notes/`)

现在创建 Cloudflare Worker，代码几乎和之前一样，只需修改映射表：

```javascript
const pathMappings = [
    { path: "/tech", upstream: "https://hugo-blog.pages.dev" },
    { path: "/life", upstream: "https://hexo-site.pages.dev" },
    { path: "/docs", upstream: "https://mkdocs-docs.pages.dev" },
    { path: "/notes", upstream: "https://jekyll-notes.pages.dev" },
    // 添加一个根路径重定向到你的主博客
    { path: "/", upstream: "https://hugo-blog.pages.dev", redirect: true }, // 新增redirect属性
];

export default {
    async fetch(request, env, ctx) {
        const url = new URL(request.url);
        const pathname = url.pathname;

        for (const mapping of pathMappings) {
            if (
                pathname.startsWith(mapping.path + "/") ||
                pathname === mapping.path
            ) {
                // 处理根路径重定向
                if (mapping.redirect && pathname === "/") {
                    return Response.redirect(
                        `${url.origin}${mapping.path}`,
                        301,
                    );
                }

                const upstreamUrl = new URL(mapping.upstream);
                upstreamUrl.pathname =
                    pathname.replace(mapping.path, "") || "/";
                upstreamUrl.search = url.search;

                const newRequest = new Request(upstreamUrl, {
                    method: request.method,
                    headers: request.headers,
                    body: request.body,
                    redirect: "manual",
                });

                let response = await fetch(newRequest);
                // ... (可选的响应头处理代码，见上一个回答)
                return response;
            }
        }
        return new Response("Page not found", { status: 404 });
    },
};
```

最后，将这个 Worker 的路由绑定到你的域名 `example.com/*`。

### 工作流程总结

至此，一个完美的自动化流程就建立了：

1.  **你写文章**：在本地 `hexo-blog` 文件夹中写作。
2.  **你推送代码**：`git add . && git commit -m "new post" && git push origin main`
3.  **自动构建**：Cloudflare Pages (或 Netlify/Vercel) 检测到推送，自动拉取代码、执行 `hexo generate`、将 `public` 文件夹部署到全球CDN。
4.  **自动更新**：用户访问 `example.com/life/new-post.html`，Cloudflare Worker 拦截请求，从 `https://hexo-site.pages.dev/new-post.html` 获取**刚刚部署的最新内容**，并返回给用户。

这个方案清晰、强大且完全免费（在免费额度内），完美解决了多 repo 源码的统一域名管理问题。

---

**目前多个repo都用github pages来托管，怎么设置路由？**

好的，既然你已经在使用 GitHub Pages 来托管多个仓库，那么这个设置会非常直接。我们只需要解决一个核心问题：**GitHub Pages 的默认 URL 结构**。

### 核心挑战与解决方案

- **挑战**：你的各个 GitHub Pages 站点的默认地址可能是：
    - `https://<你的用户名>.github.io/<repo-name>/` (对于项目站点)
    - `https://<你的用户名>.github.io/` (对于用户主页站点，只能有一个)
- **目标**：让它们都能通过 `example.com/<path>/` 访问。
- **解决方案**：使用 Cloudflare Workers 作为反向代理，并正确配置每个博客的 `baseURL`。

### 第一步：确认你的 GitHub Pages 地址

你有两种方式部署 GitHub Pages：

1.  **项目站点 (Project Site)**：每个仓库都有自己的站点，地址为 `https://<username>.github.io/<repository-name>/`。这是最常见的方式，也是我们方案的基础。
    - 例如：Hugo 博客在 `hugo-blog` 仓库，它的地址就是 `https://your-username.github.io/hugo-blog/`.

2.  **用户/组织站点 (User/Organization Site)**：需要一个名为 `<username>.github.io` 的特殊仓库，它直接部署在根路径 `https://<username>.github.io/`。**这种方式你只能有一个**。

**请确保你的所有博客都是使用“项目站点”的方式部署的**，这样每个博客才有自己独立的子路径。

### 第二步：配置每个博客的 baseURL（最关键的一步！）

你必须告诉静态网站生成器，它最终是在一个子路径下被访问的，而不是在根路径。这样它才能正确生成所有资源（CSS, JS, 图片）和内部链接的路径。

> [!important] 路径设置
> 你必须在每个博客的**源码配置文件**中，将 `baseURL` 设置为你的**最终目标路径**，即 `https://example.com/你的路径/`。

- **Hugo** (`config.toml` 或 `config.yaml`)
    ```toml
    baseURL = "https://example.com/tech/" # 注意末尾的斜杠
    ```
- **Hexo** (`_config.yml`)
    ```yaml
    url: https://example.com/life
    root: /life/ # 注意：这里开头和结尾都有斜杠
    ```
- **MkDocs** (`mkdocs.yml`)
    ```yaml
    site_url: https://example.com/docs/
    ```
- **Jekyll** (`_config.yml`)
    ```yaml
    url: "https://example.com" # 你的域名
    baseurl: "/notes" # 你想要的路径，注意没有末尾斜杠
    ```

### 第三步：创建并部署 Cloudflare Worker

这是实现无缝代理的核心。我们将创建一个 Worker，根据访问路径将请求转发到对应的 GitHub Pages 地址。

1.  **进入 Cloudflare 仪表板**，选择 **Workers & Pages**。
2.  点击 **Create Application** -> **Create Worker**。
3.  给你的 Worker 起个名字，比如 `github-pages-router`。
4.  **用以下代码替换默认代码**：

    ```javascript
    // 路径映射配置：['访问路径', '对应的GitHub Pages地址']
    const pathMappings = [
        {
            path: "/tech",
            upstream: "https://your-username.github.io/hugo-blog",
        },
        {
            path: "/life",
            upstream: "https://your-username.github.io/hexo-blog",
        },
        {
            path: "/docs",
            upstream: "https://your-username.github.io/mkdocs-project",
        },
        {
            path: "/notes",
            upstream: "https://your-username.github.io/jekyll-notes",
        },
        // 你可以继续添加更多映射...
    ];

    export default {
        async fetch(request, env, ctx) {
            const url = new URL(request.url);
            const pathname = url.pathname;

            // 1. 处理根路径重定向（可选）
            if (pathname === "/") {
                // 重定向到你的主博客，比如tech
                return Response.redirect(`${url.origin}/tech/`, 302);
            }

            // 2. 遍历映射，寻找匹配的路径
            for (const mapping of pathMappings) {
                if (
                    pathname.startsWith(mapping.path + "/") ||
                    pathname === mapping.path
                ) {
                    // 构建要请求的上游URL
                    const upstreamUrl = new URL(mapping.upstream);
                    // 将请求路径中匹配的部分替换为GitHub Pages的路径
                    // 例如：请求 /tech/posts/1 -> 代理到 https://.../hugo-blog/posts/1
                    upstreamUrl.pathname =
                        pathname.replace(
                            mapping.path,
                            mapping.upstream.pathname,
                        ) || "/";
                    upstreamUrl.search = url.search; // 保留查询参数

                    // 创建新的请求对象
                    const newRequest = new Request(upstreamUrl, {
                        method: request.method,
                        headers: request.headers,
                        body: request.body,
                        redirect: "manual", // 非常重要！由Worker来处理重定向
                    });

                    // 向上游（GitHub Pages）发送请求
                    let response = await fetch(newRequest);

                    // 3. （可选但推荐）修复重定向头
                    // 防止GitHub Pages的重定向导致用户浏览器地址栏变成 xxx.github.io
                    if (response.status >= 300 && response.status < 400) {
                        const location = response.headers.get("location");
                        if (location && location.includes(mapping.upstream)) {
                            // 将重定向地址中的GitHub URL替换为我们自己的域名和路径
                            const newLocation = location.replace(
                                mapping.upstream,
                                `${url.origin}${mapping.path}`,
                            );
                            response = new Response(response.body, response);
                            response.headers.set("location", newLocation);
                        }
                    }

                    return response;
                }
            }

            // 4. 如果没有匹配任何路径，返回404
            return new Response("Page not found!", {
                status: 404,
                headers: { "Content-Type": "text/plain" },
            });
        },
    };
    ```

    **务必修改**：
    - `your-username`：替换成你的 GitHub 用户名。
    - `hugo-blog`, `hexo-blog` 等：替换成你实际的仓库名称。

5.  **点击 "Deploy"** 部署这个 Worker。

### 第四步：绑定 Worker 到你的域名

现在需要告诉 Cloudflare：”所有对 `example.com` 的请求，都先交给这个 Worker 处理”。

1.  在你刚创建的 Worker 详情页，点击 **Triggers** -> **Add Route**。
2.  在路由框中输入：`example.com/*`
3.  点击 **Add route**。

**大功告成！**

现在你可以访问：

- `example.com/tech` -> 无缝代理到 `https://your-username.github.io/hugo-blog/`
- `example.com/life/about` -> 无缝代理到 `https://your-username.github.io/hexo-blog/about`
- `example.com/docs` -> 无缝代理到 `https://your-username.github.io/mkdocs-project/`

用户的浏览器地址栏将始终显示 `example.com`，完全感知不到背后的 GitHub Pages。

---

**如何用github自带的custom domain实现上述功能？**

当然可以！使用 GitHub Pages 自带的 **Custom Domain** 功能是实现统一域名最直接、最“官方”的方法。它本质上是一种 **CNAME 重定向** 而非代理。

但请注意，这种方法有一个**核心限制**：**一个域名只能指向一个 GitHub Pages 仓库**。你不能让 `example.com` 同时指向你的 Hugo 和 Hexo 仓库。

因此，要实现多个仓库绑定到同一域名的不同路径，我们需要采用 **子域名 (Subdomain)** 策略，而不是路径 (Path) 策略。

### 最终方案：子域名策略

我们将为每个博客仓库分配一个子域名，而不是一个路径。

- `tech.example.com` -> 指向 Hugo 博客仓库
- `life.example.com` -> 指向 Hexo 博客仓库
- `docs.example.com` -> 指向 MkDocs 仓库
- `notes.example.com` -> 指向 Jekyll 仓库
- `example.com` -> 指向你的主博客仓库（或重定向）

### 操作步骤

#### 第一步：为每个仓库配置 Custom Domain

1.  进入你的 **Hugo 博客仓库** 的 Settings -> Pages。
2.  在 **Custom domain** 输入框中，填入你为它分配的子域名，例如 `tech.example.com`。
3.  点击 **Save**。

4.  GitHub 会自动在你的仓库根目录下创建一个 `CNAME` 文件，其内容就是 `tech.example.com`。**请确保这个文件被提交并推送到你的仓库**，否则设置可能会失效。
5.  **重复以上过程**，为你的其他仓库设置对应的子域名：
    - Hexo 仓库 -> `life.example.com`
    - MkDocs 仓库 -> `docs.example.com`
    - Jekyll 仓库 -> `notes.example.com`

#### 第二步：在 DNS 提供商（Cloudflare）中添加记录

现在你需要告诉 DNS：“当有人访问 `tech.example.com` 时，请把它指向 GitHub 的服务器。”

1.  登录你的 Cloudflare 控制台，进入你的域名 `example.com` 的 DNS 管理页面。
2.  为**每个子域名**创建一条 `CNAME` 记录：
    - **Type**: `CNAME`
    - **Name**: `tech` (这代表 `tech.example.com`)
    - **Target**: `<你的用户名>.github.io` (注意：**不要**在后面加仓库名！)
    - **Proxy status**: 建议启用 (橙色云)，让 Cloudflare CDN 加速和保护你的站点。
3.  重复添加所有需要的记录：

| Type  | Name  | Target                    | Proxy Status |
| :---- | :---- | :------------------------ | :----------- |
| CNAME | tech  | `your-username.github.io` | Proxied      |
| CNAME | life  | `your-username.github.io` | Proxied      |
| CNAME | docs  | `your-username.github.io` | Proxied      |
| CNAME | notes | `your-username.github.io` | Proxied      |

#### 第三步：配置每个博客的 baseURL

你必须更新每个博客的配置文件，让它们知道现在是通过子域名访问的。

- **Hugo** (`config.toml`):
    ```toml
    baseURL = "https://tech.example.com/"
    ```
- **Hexo** (`_config.yml`):
    ```yml
    url: https://life.example.com/
    ```
- **MkDocs** (`mkdocs.yml`):
    ```yml
    site_url: https://docs.example.com/
    ```
- **Jekyll** (`_config.yml`):
    ```yml
    url: "https://notes.example.com" # 注意：这里没有末尾斜杠
    # baseurl: "" # 如果你之前设置过baseurl，请确保它为空或注释掉
    ```

### 如何处理根域名 (`example.com`)？

你只能选择一个仓库作为主站来绑定根域名。

1.  选择你最常用的博客（例如 Hugo 博客）仓库。
2.  在该仓库的 GitHub Pages 设置中，将 **Custom domain** 设置为 `example.com`（没有 `www` 前缀）。
3.  在 Cloudflare DNS 中，你需要创建以下记录：
    - **一条 `CNAME` 记录** (推荐):
        - **Type**: `CNAME`
        - **Name**: `@` (这代表根域名 `example.com`)
        - **Target**: `your-username.github.io`
        - **Proxy status**: `Proxied`
    - **或者一条 `A` 记录** (传统方法):
        - **Type**: `A`
        - **Name**: `@`
        - **Target**: `185.199.108.153` (这是 GitHub Pages 的 IP之一，有[四个](https://docs.github.com/zh/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain))
        - **Proxy status**: `Proxied`

现在，访问 `example.com` 就会打开你的 Hugo 博客。

### 总结对比

| 特性         | GitHub Custom Domain (子域名) | Cloudflare Worker (路径) |
| :----------- | :---------------------------- | :----------------------- |
| **实现方式** | DNS CNAME 记录                | 反向代理                 |
| **URL 结构** | `subdomain.example.com`       | `example.com/path`       |
| **设置难度** | **简单** (官方支持)           | 中等 (需写代码)          |
| **灵活性**   | 较低 (固定子域名)             | **极高** (可任意路由)    |
| **性能**     | 快 (直接访问)                 | 很快 (边缘网络代理)      |
| **推荐度**   | **推荐** (简单直观)           | **推荐** (功能强大)      |

**建议**：如果你可以接受子域名结构（`tech.example.com`），那么使用 GitHub 自带的 Custom Domain 功能是**最简单、最稳定**的选择。如果你坚持要使用路径结构（`example.com/tech`），那么 Cloudflare Worker 是**唯一可行**的方案。

---

**我想要在一个仓库里管理所有具体内容，也就是markdown文件。然后根据所在的文件夹用hexo hugo等分别构建静态文件，然后再host在一个域名下。我希望我的内容仓库只包含具体博客的markdown文件，不包括配置文件或附件。**

### 最终架构图

```
[你的内容仓库] (只含Markdown和assets)
       |
       | (GitHub Actions 触发)
       |
       V
+-------------------------------+
|        构建矩阵 (CI/CD)        |
| +---------+  +---------+      |
| | Hugo CI |  | Hexo CI | ...  | --> [构建后的静态文件]
| +---------+  +---------+      |
+-------------------------------+
       |
       | (部署)
       V
[Cloudflare Pages] 或 [Cloudflare R2] + [Worker 路由]
       |
       V
用户访问 example.com/tech/ --> Hugo 博客
         example.com/life/ --> Hexo 博客
```

### 第一步：创建并组织你的内容仓库

假设你的仓库名为 `my-blog-content`。

**仓库结构建议：**

```
my-blog-content/
├── .github/workflows/     # GitHub Actions 工作流文件
├── tech/                  # Hugo 博客内容
├── life/                  # Hexo 博客内容
├── docs/                  # MkDocs 文档内容
└── shared-assets/         # 可跨博客使用的共享资源（可选）
```

**关键点**：

- **不包含** `_config.yml`, `themes/`, `package.json` 等任何生成器配置文件或依赖。
- 每个子目录（`tech/`, `life/`, `docs/`）的结构**模拟**了对应生成器所期望的源文件结构。

### 第二步：为每个生成器创建配置仓库（可选但推荐）

为了保持纯粹，你可以为 Hugo, Hexo, MkDocs 分别创建独立的**配置仓库**，例如：

- `my-blog-hugo-config`
- `my-blog-hexo-config`
- `my-blog-mkdocs-config`

这些仓库**只包含**主题、配置文件、插件等，**不包含**任何文章内容。

```
my-blog-hugo-config/
├── archetypes/
├── assets/
├── layouts/
├── static/
├── themes/          # 你的主题，或作为git子模块引入
└── config.toml      # 关键：baseURL = "https://example.com/tech/"
```

在构建时，GitHub Actions 会同时拉取**内容仓库**和**配置仓库**，将它们合并后进行构建。

### 第三步：编写 GitHub Actions 工作流（核心）

这是在 `.github/workflows/deploy.yml` 中的自动化脚本。它将完成拉取内容、拉取配置、构建、部署的全过程。

```yaml
name: Deploy All Blogs

on:
    push:
        branches: ["main"]
    # 允许手动触发
    workflow_dispatch:

jobs:
    build-and-deploy:
        runs-on: ubuntu-latest
        strategy:
            matrix:
                # 定义构建矩阵，每个生成器一个任务
                generator: [hugo, hexo, mkdocs]
        steps:
            - name: Checkout Content Repo
              uses: actions/checkout@v4
              with:
                  path: "content"

            - name: Checkout Config Repo
              uses: actions/checkout@v4
              with:
                  repository: "your-username/my-blog-${{ matrix.generator }}-config" # 拉取对应的配置仓库
                  path: "config"

            - name: Setup Node.js (for Hexo)
              if: matrix.generator == 'hexo'
              uses: actions/setup-node@v4
              with:
                  node-version: "20"

            - name: Setup Python (for MkDocs)
              if: matrix.generator == 'mkdocs'
              uses: actions/setup-python@v5
              with:
                  python-version: "3.x"

            - name: Prepare Build Environment
              run: |
                  # 将内容和配置合并到构建目录
                  mkdir -p build/${{ matrix.generator }}
                  cp -r config/. build/${{ matrix.generator }}/
                  cp -r content/${{ matrix.generator }}/. build/${{ matrix.generator }}/ 2>/dev/null || :

                  # 对于Hexo，需要将content/life/_posts复制到source/_posts
                  if [ "${{ matrix.generator }}" = "hexo" ]; then
                    cp -r content/life/_posts build/hexo/source/
                    cp -r content/life/images build/hexo/source/
                  fi

                  # 对于Hugo，content/tech/已经是正确的源文件路径
                  # 对于MkDocs，content/docs/docs/需要是mkdocs.yml中docs_dir指向的路径

            - name: Build with Hugo
              if: matrix.generator == 'hugo'
              working-directory: ./build/hugo
              run: |
                  wget https://github.com/gohugoio/hugo/releases/download/v0.125.0/hugo_extended_0.125.0_linux-amd64.deb
                  sudo dpkg -i hugo_extended_0.125.0_linux-amd64.deb
                  hugo --minify

            - name: Build with Hexo
              if: matrix.generator == 'hexo'
              working-directory: ./build/hexo
              run: |
                  npm install
                  npx hexo generate

            - name: Build with MkDocs
              if: matrix.generator == 'mkdocs'
              working-directory: ./build/mkdocs
              run: |
                  pip install mkdocs-material
                  mkdocs build

            - name: Upload Artifact
              uses: actions/upload-pages-artifact@v3
              with:
                  name: ${{ matrix.generator }}-site
                  path: |
                      build/hugo/public/
                      build/hexo/public/
                      build/mkdocs/site/

    deploy:
        runs-on: ubuntu-latest
        needs: build-and-deploy
        permissions:
            pages: write
            id-token: write
        environment:
            name: github-pages
            url: ${{ steps.deployment.outputs.page_url }}
        steps:
            - name: Deploy to Cloudflare Pages
              uses: cloudflare/pages-action@v1
              with:
                  apiToken: ${{ secrets.CLOUDFLARE_API_TOKEN }}
                  accountId: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
                  projectName: my-unified-blog
                  directory: . # 注意：这里需要调整，因为 artifacts 是分开的
                  # 更复杂的部署可能需要自定义脚本
```

**注意**：上面的工作流是一个复杂的概念验证。在实际生产中，你可能会选择：

1.  **为每个生成器创建独立的工作流文件**（如 `deploy-hugo.yml`, `deploy-hexo.yml`），这样更清晰。
2.  或者使用 **`needs`** 关键字创建依赖关系图，让它们并行构建。
3.  部署步骤可能需要先将所有构建好的静态文件（`hugo/public/`, `hexo/public/`, `mkdocs/site/`）收集到一个总目录（如 `combined-site/`）下，再进行一次性部署。

### 第四步：配置 Cloudflare 进行路由和托管

你有两个主要的托管选择：

#### 选项A：Cloudflare Pages（最简单）

1.  按照上面的工作流，将所有构建好的静态文件部署到同一个 Cloudflare Pages 项目。
2.  项目的文件结构在部署后应该是：
    ```
    /
    ├── tech/    # Hugo 生成的整个public文件夹内容
    ├── life/    # Hexo 生成的整个public文件夹内容
    └── docs/    # MkDocs 生成的整个site文件夹内容
    ```

### 总结

这个方案实现了你的所有目标：

1.  **内容与配置分离**：一个纯净的 `markdown` 和 `assets` 仓库。
2.  **多生成器构建**：利用 GitHub Actions 矩阵策略并行处理。
3.  **统一域名托管**：通过 Cloudflare Pages 或 Pages + Worker 实现。

虽然初始设置比较复杂，但一旦完成，你的工作流将变得非常清晰和强大：**你只需要在一个地方写 `markdown` 和推送，所有博客都会自动更新。**
