---
title: ArchWiki摘抄
toc: true
tags:
    - arch
    - linux
    - handbook
date: 2025-01-03
dg-publish: true
---

# ArchWiki摘抄

## pacman

```sh
pacman -Qqe | fzf --preview 'pacman -Qiil {}' --layout=reverse --bind 'enter:execute(pacman -Qiil {} | less)'
```

```sh
pacman -Slq | fzf --preview 'pacman -Si {}' --layout=reverse
```

```sh
pacman -D --asdeps $(pacman -Qqe)
```

```sh
pacman -D --asexplicit base linux linux-firmware
```

```sh
cat explicit | sudo pacman -D --asexplicit -
```

```sh
pacman -Qii | awk '/^MODIFIED/ {print $2}'
```

```sh
pacman -Qi | awk '/^Name/{name=$3} /^Install Date/{print $4,$5,$6,name}' | sort
```

## font

find the font that contains chinese

```sh
fc-list -f '%{file}\n' :lang=zh
```

check monospace

```sh
fc-match monospace
```

set fallback fonts, edit `$XDG_CONFIG_HOME/fontconfig/fonts.conf`:

```text
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
<alias>
   <family>serif</family>
   <prefer>
     <family>LXGW Wenkai mono</family>
   </prefer>
 </alias>
<alias>
   <family>monospace</family>
   <prefer>
     <family>CodeNewRoman Nerd Font</family>
     <family>LXGW Wenkai mono</family>
   </prefer>
 </alias>
</fontconfig>

```
