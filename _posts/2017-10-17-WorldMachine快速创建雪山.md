---
layout:     post
title:      WorldMachine 快速实现雪山
subtitle:   WorldMachine
date:       2017-10-17
author:     Trump
header-img: img/post-bg-debug.png
catalog: true
tags:
- WorldMachine
---


# WorldMachine 快速实现雪山
## 效果图

![](http://mingchuan.wang/img/WM_SnowMountain/1.png)
![](http://mingchuan.wang/img/WM_SnowMountain/2.png)

## 实现代码和下载链接如下

![](http://mingchuan.wang/img/WM_SnowMountain/3.png)

每个节点就不一一讲解了毕竟不是WorldMachine教程，源文件如下：

文件下载链接：https://pan.baidu.com/s/18vgGas42MzV9tvuwc7YMXg 中的 SingleSnowMountainOut.tmd

源文件的效果可能和上面两张图有所出入，可以通过调整上面的箭头的mask来修改。

另外可以选择不同的分辨率导出两个Mesh，分别对应高模和低模，烘焙出法线贴图，在根据splatmap图上材质，就可以作为StaticMesh导入引擎中使用而不是高度图，这样根据需求使用很灵活。



版权声明：本文为博主原创文章，未经博主允许不得转载。
