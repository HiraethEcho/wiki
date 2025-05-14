---
title: linux-daily
date:
    created: 2025-01-19
---

# 每天一点linux知识

## 2025-05-14

arch的`ufrii-print`out of date，官网有新驱动。手动修改pkgbuild可以装上去。但是其中一个依赖`libxml2`版本版本过新不匹配，还是打印不了。最后装了`libxml2-legacy`，新驱动和旧驱动都能打印。  
解决后第二天`libxml2`就又有更新，不知道解决没有，懒得测试了。

出现这种问题的时候就真的会想debian养老。

## 2025-04-24

骑马钉打印文件，A4pdf文件，每页A5大小打印在A4纸上，对折翻页。在arch可以用`pdfbooklet`。  
但是打印的时候不设置的话会一页纸正反面方向出问题。按理说设置成landscape后短边反转就可以了，但是学校打印机抽风搞不定。  
所以再手动调整，先全部旋转（rotate）90 degree，（不是翻转flip！这个是镜像，字会反过来！），再所有偶数页旋转180。再打印。
