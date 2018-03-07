---
layout:     post
title:      My Indie Game Diary 第二篇
subtitle:   高模低模导出到Substance Painter流程
date:       2018-2-2
author:     Trum
header-img: img/post-bg-debug.png
catalog: true
tags:
- substance Painter
- 3dmax
---

# My Indie Game Diary 第二篇 高模低模导入Substance Painter流程
## 准备工作
- 首先在3dmax（或Maya）中准备好高模和低模，由于模型还在完善过程中，模型文件我就先不分享了，做好了统一分享。
- 低模分好UV（我使用的是Unfold3D，非常高效）,在层管理器中,将低模分在一个层中，并将每个模型名称后缀加上"_low"。
- 高模部分，在层管理器中,将高模分在一个层中,将每个模型名称后加上"_high"。

![](http://mingchuan.wang/img/MyIndieGameDiary/1.png)

注意：1.给后缀改名添加"_low"和"_high"是因为在substance Painter 中烘焙贴图，substance Painter 可以根据后缀名来区分模型部分那个是高模那个是低模，按名称匹配，这样定位会更准确。 
2."_low"和"_high"不要写错了，一定要和substance中相对应（本人曾经在这错把high写成height...浪费了好长时间 -_-!）。
## 高模赋予材质
- 高模赋予材质的目的是，在substance Painter中会有烘焙Id的功能，可根据材质颜色来烘焙出一个ID贴图，这个贴图作用是方便你想要为模型那一部分单独上材质做区分，可通过pick color的功能指定上材质区域，在画的时候不会影响到其他的地方。因为是根据材质颜色来区分的，所以在3dmax中给高模上的材质颜色应尽量的不同。

![](http://mingchuan.wang/img/MyIndieGameDiary_2/9.png)

## 导出低模
- 选中低模部的所有模型，导出选中对象

![](http://mingchuan.wang/img/MyIndieGameDiary_2/2.png)
![](http://mingchuan.wang/img/MyIndieGameDiary_2/3.png)

- 导出配置

![](http://mingchuan.wang/img/MyIndieGameDiary_2/4.png)
![](http://mingchuan.wang/img/MyIndieGameDiary_2/5.png)

## 导出高模

- 选中高模部的所有模型

![](http://mingchuan.wang/img/MyIndieGameDiary_2/6.png)

- 导出配置

![](http://mingchuan.wang/img/MyIndieGameDiary_2/8.png)
![](http://mingchuan.wang/img/MyIndieGameDiary_2/7.png)

