---
title: 从键盘到字符
toc: true
tags:
  - linux
date: 2024-12-17
status: draft
moved: true
---

# 从键盘到字符

软件版

计算机从键盘收到信号后发生了什么

## Preliminaries

### keyboard:

find id of touchpad:

```
xinput list | grep -i "Touchpad" | awk '{print $6}' | sed 's/[^0-9]//g'
```

似乎需要添加文件 `/usr/share/X11/xorg.conf.d/30-touchpad.conf`

```conf
Section "InputClass"
        Identifier "MyTouchpad"
        MatchIsTouchpad "on"
        Driver "libinput"
        Option "Tapping" "on"
EndSection
```

之后才能正常识别`touchpad`：

```xinput list

⎡ Virtual core pointer                    	id=2	[master pointer  (3)]
⎜   ↳ Virtual core XTEST pointer              	id=4	[slave  pointer  (2)]
⎜   ↳ GXTP7863:00 27C6:01E0 Mouse             	id=11	[slave  pointer  (2)]
⎜   ↳ GXTP7863:00 27C6:01E0 Touchpad          	id=12	[slave  pointer  (2)]
⎜   ↳ HS6209 2.4G Wireless Receiver Keyboard  	id=9	[slave  pointer  (2)]
⎜   ↳ HS6209 2.4G Wireless Receiver Mouse     	id=10	[slave  pointer  (2)]
⎣ Virtual core keyboard                   	id=3	[master keyboard (2)]
    ↳ Virtual core XTEST keyboard             	id=5	[slave  keyboard (3)]
    ↳ Video Bus                               	id=6	[slave  keyboard (3)]
    ↳ Power Button                            	id=7	[slave  keyboard (3)]
    ↳ Huawei WMI hotkeys                      	id=13	[slave  keyboard (3)]
    ↳ AT Translated Set 2 keyboard            	id=14	[slave  keyboard (3)]
```

否则只会识别两个设备，没有 `Mouse` `Touchpad`

```xinput list
⎡ Virtual core pointer                    	id=2	[master pointer  (3)]
⎜   ↳ Virtual core XTEST pointer              	id=4	[slave  pointer  (2)]
⎜   ↳ GXTP7863:00 27C6:01E0                   	id=10	[slave  pointer  (2)]
⎜   ↳ GXTP7863:00 27C6:01E0                   	id=11	[slave  pointer  (2)]
⎣ Virtual core keyboard                   	id=3	[master keyboard (2)]
    ↳ Virtual core XTEST keyboard             	id=5	[slave  keyboard (3)]
    ↳ Video Bus                               	id=6	[slave  keyboard (3)]
    ↳ Power Button                            	id=7	[slave  keyboard (3)]
    ↳ Huawei WMI hotkeys                      	id=12	[slave  keyboard (3)]
    ↳ AT Translated Set 2 keyboard            	id=13	[slave  keyboard (3)]
```

`xev`: find keycode; `xmodmap`: define keysym for each keycode (`xkeycaps` gui); `setxkbmap`: set keyboard layout; `xcape`: use a modifier as another key.

for example, <kbd>caps</kbd> as <kbd>escape</kbd> and <kbd>ctrl</kbd>

```sh
setxkbmap -option ctrl:swapcaps # swap ctrl and caps
setxkbmap -option ctrl:nocaps
xcape -e 'Alt_L=Escape' # Escape when tap, ALt when hold
# I usually use following
setxkbmap -option 'caps:ctrl_modifier' # caps become ctrl
xcape -e 'Caps_Lock=Escape'
```

usage of `xmodmap`:

modifier for X Window. mod1: left Alt, mod2: Num_Lock，mod3: no，mod4: Left Super (windows)，mod5: Shift

```xmodmap -pm

xmodmap:  up to 4 keys per modifier, (keycodes in parentheses):

shift       Shift_L (0x32),  Shift_R (0x3e)
lock        Caps_Lock (0x42)
control     Control_L (0x25),  Control_R (0x69)
mod1        Alt_L (0x40),  Alt_R (0x6c),  Alt_L (0xcc),  Meta_L (0xcd)
mod2        Num_Lock (0x4d)
mod3        ISO_Level5_Shift (0xcb),  Hyper_L (0xcf)
mod4        Super_L (0x85),  Super_R (0x86),  Super_L (0xce)
mod5        ISO_Level3_Shift (0x5c)
```

Keycode is what the kernal get, and keysym is what it gives. Each keysym column in the table corresponds to a particular combination of modifier keys:

1. Key
2. Shift+Key
3. Mode_switch+Key
4. Mode_switch+Shift+Key
5. ISO_Level3_Shift+Key
6. ISO_Level3_Shift+Shift+Key

check current keycode map:

```xmodmap -pke

keycode   8 =
keycode   9 = Escape NoSymbol Escape
keycode  10 = 1 exclam 1 exclam
keycode  11 = 2 at 2 at
keycode  12 = 3 numbersign 3 numbersign
keycode  13 = 4 dollar 4 dollar
keycode  14 = 5 percent 5 percent
keycode  15 = 6 asciicircum 6 asciicircum
keycode  16 = 7 ampersand 7 ampersand
keycode  17 = 8 asterisk 8 asterisk
keycode  18 = 9 parenleft 9 parenleft
keycode  19 = 0 parenright 0 parenright
...
keycode  34 = bracketleft braceleft bracketleft braceleft
keycode  35 = bracketright braceright bracketright braceright
keycode  36 = Return NoSymbol Return
keycode  37 = Control_L NoSymbol Control_L
keycode  38 = a A a A
keycode  39 = s S s S
keycode  40 = d D d D
keycode  41 = f F f F
keycode  42 = g G g G
keycode  43 = h H h H
keycode  67 = F1 F1 F1 F1 F1 F1 XF86Switch_VT_1
keycode  68 = F2 F2 F2 F2 F2 F2 XF86Switch_VT_2
...
```

[ref](https://unix.stackexchange.com/questions/55076/what-is-the-mode-switch-modifier-for/55154#55154)

another softwares:

- interception-tools with plugins
    - dual-function-keys (like Mod-Tap feature of zmk and qmk)
    - caps2esc
    - space2meta
- [keyd](https://github.com/rvaiya/keyd): this is sick.

# Reference

- archwiki:
    - [https://wiki.archlinux.org/title/Keyboard_input](https://wiki.archlinux.org/title/Keyboard_input)
