---
title: nvim的技巧
toc: true
tags:
date: 2025-07-08
---

# Tricks in nvim

## start up of nvim

[nvim doc](https://neovim.io/doc/user/starting.html#_initialization)

## lazy.nvim

[ref](https://docs.astronvim.com/configuration/customizing_plugins/) `lazy.nvim` 配置的表合并情况。例如假设`treesitter`的默认配置为

```lua
return {
  "nvim-treesitter/nvim-treesitter",
  opts = {
    ensure_installed = { "lua", "vim" },
    highlight = {
      enable = true,
    },
  },
}
```

手动配置

```lua
return {
  "nvim-treesitter/nvim-treesitter",
  opts = {
    ensure_installed = { "python" },
    highlight = {
      enable = false,
    },
    indent = {
      enable = false,
    },
  },
}
```

那么最后会得到`ensure_installed = { "python" }`。如何要拓展表，则要用

```lua
table.insert(opts.ensure_installed, "python")
```
