---
dg-publish: false
title: 本站和markdown的语法
gists:
  - id: 1748bf8ea8f33ab9634a912240aa81d2
    url: 'https://gist.github.com/HiraethEcho/1748bf8ea8f33ab9634a912240aa81d2'
    createdAt: '2025-05-23T12:40:18Z'
    updatedAt: '2025-05-28T13:31:10Z'
    filename: examples.md
    isPublic: true
    baseUrl: 'https://api.github.com'
---

# 本站和markdown的语法

## Text Formatting

```md
- [ ] ==Get==
- [-] things
- [x] ~~done~~
```

- [ ] ==Get==
- [-] things
- [x] ~~done~~

More formatting options for your webpage
[here](https://squidfunk.github.io/mkdocs-material/reference/formatting/#highlighting-changes).
(but not compatible with Obsidian)

```callout
> [!note]
> callout tests

> [!tip]
> tips
```

> [!note] callout tests

> [!tip] tips

```html
<div class="admonition note">
    <p class="admonition-title">Note</p>
    <p>test</p>
</div>
```

<div class="admonition note">
    <p class="admonition-title">Note</p>
<p>test</p>
</div>

    block?
    and another line

```md
!!! note test !!! tip tips

!!! example example !!! danger !!! cite !!! question !!! summary ??? warning
this caution
```

!!! note test !!! tip tips

!!! example example !!! danger !!! cite !!! question !!! summary ??? warning
this caution

## link

```md
[wiki](wiki)  
[wiki/code/arch/archinstall](wiki/code/arch/archinstall)  
[/wiki](/wiki)  
[/wiki/code/arch/archinstall](/wiki/code/arch/archinstall)  
![wiki/img/a.png](wiki/img/a.png) ![/wiki/img/a.png](/wiki/img/a.png)
```

[wiki](wiki) https://hiraethecho.github.io/wiki/code/cheatsheets/examples/wiki
[wiki/code/arch/archinstall](wiki/code/arch/archinstall) as above [/wiki](/wiki)
yes [/wiki/code/archinstall](/wiki/code/arch/archinstall) yes
![wiki/img/a.png](wiki/img/a.png) plain text ![/wiki/img/a.png](/wiki/img/a.png)

## Mermaid diagrams

Here's the example from
[MkDocs Material documentation](https://squidfunk.github.io/mkdocs-material/reference/diagrams/#using-flowcharts):

```mermaid
  graph LR
  A[Start] --> B{Error?}; B -->|Yes| C[Hmm...];
  C --> D[Debug];
  D --> B;
  B ---->|No| E[Yay!];
```

## LaTeX Math Support

LaTeX math is supported using MathJax.

Inline math looks like $f(x) = x^2$. The input for this is `$f(x) = x^2$`. Use
`$...$`.  
For a block of math, use `$$...$$` on separate lines

```
$$
F(x) = \int^a_b \frac{1}{2}x^4
$$
```
