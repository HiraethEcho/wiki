---
title: 折腾的linux+X11+dwm
categories:
    - linux
tags:
    - linux
    - daily
published: true
date:
    created: 2024-07-29
excerpt: 关于linux C dwm等
---

# 近期折腾的archlinux

也算是一些业余学习吧。先叠个甲，业余学习所以可能有错误，只是个人理解。

<!-- more -->

## linux

linux内核的功能是调用硬件，为程序分配使用  
GNU is not unix. 在斯托曼的组织下，为了取代unix开发了一系列自由软件，但是没有内核，用了linus的内核。  
shell是包裹在内核外面的外壳，用来和内核沟通。并且有POSIX的shell标准，来规范shell的语言。常见的shell有`bash` `zsh`等。而另一个常见的`fish`不符合POSIX.  
Terminal是使用shell的界面。最基础的terminal就是tty，是过去计算机的一个物理部件（console控制台是另一个过去的物理部件，直接控制机器。tty是*远程*的控制方式。）常见的terminal有alacritty、kitty和各大桌面环境配套的。

linux的哲学之一在于，每一个程序只做一件事，并努力做到最好。像word这样的windows应用，常被认为是臃肿的，“每个人只用到了全部功能的1%
”。但这是因为“每个人用的都是不同的1%”。然而在linux的世界里，为了自己需要的那1%，通常只需要安装对应的那1%。当然，困难之处在于在广大社区中找到对应的程序，并且使自己所需要的各种程序和谐共存互相协调。

GNU/linux有各种发行版，虽然个人感到主要区别在包管理器和预装软件。包管理器的其中一个功能就是尽量保证各个程序可以互相协调，自动解决程序的依赖等问题。至于预装软件可能是发行版制作者的个人倾向。  
BTW，I use arch.

Arch发行版是一个Geek风格的linux发行版，目的在于用户尽可能自由配置自己的linux（当然似乎比不上Gentoo甚至lfs）。它有两个特点，其一是滚动更新，即没有稳定版本，总可以获得最新的软件版本。好处是获得最新功能，第一时间获得已知bug的修复；坏处是，也可以第一时间获得bug，以及面临软件之间版本不兼容的问题（这也是linux哲学的负面影响）。第二个特点是AUR，既用户仓库。每个发行版的包管理器通常有对应的软件源，而arch除了官方软件源外还有用户仓库，即每个用户都可以把应用按照规范打包供他人使用。优点在于AUR里什么都有，缺点在于很多应用无人负责或没有充分维护。

## 图形化界面X11

为了在tty外情景使用linux，需要图形化界面。

X11是linux的一个图形化界面的协议，每个图形化程序并不是靠它自己显示画面，而是通过协议用Xorg来创建图形的。除此之外还有混成器（例如picom）来提供一些画面特效等。wayland是另一套图形化方案，本身替代了Xorg和混成器的功能。

linux下的桌面环境（Desktop Environment），如KDE GNOME Xfce等，提供了一整套日常使用的基本功能，例如窗口管理，应用启动器，托盘，设置，蓝牙，网络等。但事实上如果追求极简，那么仅仅使用窗口管理器（Windows Manager）也是可行的。

Windows Manager，通俗的解释，就是显示应用程序的窗口，并且拖动、调整大小、最大化等功能的部分，不包括壁纸、声音网络蓝牙键盘触摸板等设置、应用启动器和托盘。  
常见的独立窗口管理器包括i3，openbox，awesome等。而我选择的是dwm，属于suckless系列，用C语言编写，代码仅2000余行，更改配置需要修改源码重新编译，添加功能是通过.diff文件打补丁实现。

## dwm和C

这几周时间一直在加强的我dwm，被迫学习了C语言的使用，以及基于lazygit的自娱自乐式历史管理。

C语言是面向过程的，以函数为中心的。但实际写dwm代码的时候，则主要用“面向对象”的思路在写。对象里的函数和变量是用指针或者全局变量，实现地很曲折。

C语言算是很底层的编程语言了，指针可能是其最重要的特性。指针直接指向变量的存储地址，非常有趣。在函数调用时，传参传指针进去，可以直接在物理存贮的意义上理解程序是如何使用和修改变量的值的。另外还有位运算，也很有趣。

C语言同时还是非常省内存（这里应该指的是内存吗？）的语言，每次写一个新变量都小心翼翼地。遥想大一学C语言的时候，还要注意变量要在代码块之前先声明，开个动态数组都麻烦的一匹，用完还要free掉。后来看见其他语言随便新建变量，太难接受了。不过这种节省也导致一些逆天的feature.例如一个结构体

```C
typedef struct {
    int i;
    float f;
} Arg1;
```

那么一个Arg就会有两个成员。但是还有一种操作

```C
typedef union {
    int i;
    float f;
    void *v;
} Arg2;
```

乍一看和上一个差不多，但这个`union`的意思是，`Arg2`只能有一个类型。当创建一个`Arg1 a1`时，会给 `a1.i`和`a1.f`都分配地址。但是`Arg2 a2`就只有一个地址，当用了`a2.i=1`之后就不能再赋值`a2.f`了。这样如果不同函数的输入一个是`int`另一个是`float`，那就可以都写成`Arg2`，十分优雅。但是需要复杂功能是就很难用。  
除此之外，这个`void *v`也是个奇妙的用法，只要用上强制类型转换`Client *c=(Client *)v`就可以很灵活的使用各种类型的变量了。

##

最后，可以在我的[github](https://github.com/hiraethecho)找到我的dotfiles和dwm。还没有整理，也没文档，以后有时间心情再说吧。

## tabbed scratchpad

`st -c class -n name` with `precmd () {print -Pn "\e]0;%~\a"}` in `.zshrc`

```
_NET_WM_ICON_NAME(UTF8_STRING) = "~"
WM_ICON_NAME(UTF8_STRING) = "~"
WM_CLASS(STRING) = "name", "class"
_NET_WM_NAME(UTF8_STRING) = "~"
WM_NAME(UTF8_STRING) = "~"
```

`tabbed -n "scratchpad" -c -r 2 st -w '' -g 150x40 -c class -n name` with `precmd () {print -Pn "\e]0;%~\a"}` in `.zshrc`

```
WM_NAME(UTF8_STRING) = "~"
_NET_WM_NAME(UTF8_STRING) = "~"
WM_CLASS(STRING) = "scratchpad", "tabbed"
```

**summary**: In `WM_CLASS(STRING)="name","class"`, `"name"` is for `st -n name` and `tabbed -n name`, and is also `instance` in `rules`; `class` is for `st -c class`, and class of `tabbed` is always `tabbed`.


origin `manage()`:

```
  selmon->tagset[selmon->seltags] &= ~scratchtag;
  if (!strcmp(c->name, scratchpadname)) {
    c->mon->tagset[c->mon->seltags] |= c->tags = scratchtag;
    c->isfloating = True;
  }
```
we can modify to

```
void manage(Window w, XWindowAttributes *wa) {
  Client *c, *t = NULL;
+ XClassHint ch = {NULL, NULL};
+ const char *class, *instance;
    ...
+ XGetClassHint(dpy, c->win, &ch);
+  instance = ch.res_name ? ch.res_name : broken;
  selmon->tagset[selmon->seltags] &= ~scratchtag;
~ if (!strcmp(instance, scratchpadname)) {
    c->mon->tagset[c->mon->seltags] |= c->tags = scratchtag;
    c->isfloating = True;
  }
}
```

btw, `apply_rules`:

```
  XGetClassHint(dpy, c->win, &ch);
  class = ch.res_class ? ch.res_class : broken;
  instance = ch.res_name ? ch.res_name : broken;

  for (i = 0; i < LENGTH(rules); i++) {
    r = &rules[i];
    if ((!r->title || strstr(c->name, r->title)) &&
        (!r->class || strstr(class, r->class)) &&
        (!r->instance || strstr(instance, r->instance))) {
      c->isfloating = r->isfloating;
      c->tags |= r->tags;
      c->opacity = r->opacity;
      c->unfocusopacity = r->unfocusopacity;
      for (m = mons; m && m->num != r->monitor; m = m->next) ;
      if (m)
        c->mon = m;
    }
  }
```

