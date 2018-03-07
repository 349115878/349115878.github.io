---
layout:     post
title:      My Indie Game Diary 第三篇
subtitle:   Substance Painter烘焙贴图流程
date:       2018-2-18
author:     Trum
header-img: img/post-bg-debug.png
catalog: true
tags:
- substance Painter
---

# My Indie Game Diary 第三篇 Substance Painter烘焙贴图流程
## 准备工作
- 高模和低模我们在《My Indie Game Diary 第二篇》中已经导出了，那么可以再substance painter中烘焙了。
## 新建substance Painter 工程

![](http://mingchuan.wang/img/MyIndieGameDiary_3/3.png)

- 上图1模板，可以根据你的项目需要选择含有不同PBR材质属性的模板。

![](http://mingchuan.wang/img/MyIndieGameDiary_3/2.png)

- 上图2点击选择添加准备好的低模。
- 上图3点击，贴图分辨率后面可以调整可以先不用管。
- 点击OK导入。

## 烘焙一个基础法线贴图
导入成功之后如下图

![](http://mingchuan.wang/img/MyIndieGameDiary_3/4.png)

右上角你会发现有两个材质，这两个材质就是低模上赋予的材质。会生成两套贴图。

点击BackTextures按钮进行烘焙,这一步是为了验证一下模型是否能烘焙成功，如果不成功，那就需要回头看看哪一步设置错了（比如Mesh的后缀名字是不是 "_high","_low"）。

![](http://mingchuan.wang/img/MyIndieGameDiary_3/6.png)

- 上图1，可以设置贴图分分辨率。
- 上图2，可以UV的容差值，建议设置大一些 16。
- 上图3，添加高模。

![](http://mingchuan.wang/img/MyIndieGameDiary_3/5.png)

- 上图4，设置远近边。
- 上图5，根据名字匹配，这就是在3dmax 中为高低模定义后缀名"_high","_low"。
- 上图6，只选择烘焙normal。
- 上图7，烘焙所有贴图集，这里我们在低模材质定义了个两个，这两个同时烘焙，substance painter 更老的版本没有之歌功能，那只能一个个烘焙了。

![](http://mingchuan.wang/img/MyIndieGameDiary_3/7.png)

烘焙成功之后如上图，可以点击右上角切换另一套材质查看

## 
