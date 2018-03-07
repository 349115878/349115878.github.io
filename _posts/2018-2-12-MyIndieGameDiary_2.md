---
layout:     post
title:      My Indie Game Diary 第二篇
subtitle:   3dmax导出高模低模(到Substance Painter烘焙)流程
date:       2018-2-2
author:     Trum
header-img: img/post-bg-debug.png
catalog: true
tags:
- substance Painter
- 3dmax
---

# My Indie Game Diary 第二篇 3dmax导出高模低模(到Substance Painter烘焙)流程

## 准备工作
- 首先在3dmax（或Maya）中准备好高模和低模，由于模型还在完善过程中，模型文件我就先不分享了，做好了统一分享。
- 低模分好UV（我使用的是Unfold3D，非常高效）,在层管理器中,将低模分在一个层中，并将每个模型名称后缀加上"_low"。
- 高模部分，在层管理器中,将高模分在一个层中,将每个模型名称后加上"_high"。

![](http://mingchuan.wang/img/MyIndieGameDiary_2/1.png)

注意：1.给后缀改名添加"_low"和"_high"是因为在substance Painter 中烘焙贴图，substance Painter 可以根据后缀名来区分模型部分那个是高模那个是低模，按名称匹配，这样定位会更准确。 
2."_low"和"_high"不要写错了，一定要和substance中相对应（本人曾经在这错把high写成height...浪费了好长时间 -_-!）。

## 低模赋予材质
- 低模赋予材质很好理解，根据需求你的模型最终在游戏引擎中需要以那几部分材质实现（大多数都是一张贴图）。以我的这个模型为例子，我需要这个小怪在不同的情况（战斗，警惕，巡逻）下眼睛有不同的表现，这时就需要将该部分模型单独分离出来上材质，在游戏引擎中通过参数修改该材质，从而表现出眼睛不同的状态。代价是游戏中多了几张贴图，这样的需求还是少点为好。

![](http://mingchuan.wang/img/MyIndieGameDiary_2/10.png)

## 高模赋予材质
- 高模赋予材质的目的是，在substance Painter中会有烘焙Id的功能，可根据材质颜色来烘焙出一个ID贴图，这个贴图作用是方便你想要为模型那一部分单独上材质做区分，可通过pick color的功能指定上材质区域，在画的时候不会影响到其他的地方。因为是根据材质颜色来区分的，所以在3dmax中给高模上的材质颜色应尽量的不同。

![](http://mingchuan.wang/img/MyIndieGameDiary_2/9.png)

## 导出低模
- 选中低模部的所有模型，导出选中对象

![](http://mingchuan.wang/img/MyIndieGameDiary_2/2.png)
![](http://mingchuan.wang/img/MyIndieGameDiary_2/3.png)

- 导出配置

![](http://mingchuan.wang/img/MyIndieGameDiary_2/4.png)

- 导出路径和名字自己根据需要设置。

![](http://mingchuan.wang/img/MyIndieGameDiary_2/5.png)

## 导出高模

- 选中高模部的所有模型

![](http://mingchuan.wang/img/MyIndieGameDiary_2/6.png)

- 导出配置

![](http://mingchuan.wang/img/MyIndieGameDiary_2/8.png)

- 导出路径和名字自己根据需要设置，同上。

![](http://mingchuan.wang/img/MyIndieGameDiary_2/7.png)

## 结束

- 完成之后在你导出的路径下就有两个文件了，一个高模一个低模，在下一篇 《My Indie Game Diary 第二篇》中展示在substance painter 中用这两个高低模烘焙贴图的流程。
- 上面的小怪模型是本人空余时间做的，是本人有生以来做的第一个模型，有什么建议可以发我邮箱，349115878@qq.com（吐槽一下，建模好麻烦啊~），还在调整之中模型文件先不分享了哈~~~

