---
layout:     post
title:      UE4 4.9版本 DLC 重新挂载
subtitle:   DLC 热更新 Pak
date:       2017-02-16
author:     BY
header-img: img/post-bg-debug.png
catalog: true
tags:
    - Mac
    - 终端
    - Git
---

# UE4 Procedural Mesh 实现顶角线地脚线效果

## 需求
#### 1.一键生成围绕墙一圈的顶角线和地脚线
#### 2.选中一段（或全选）更换材质，删除。
#### 3.任意调整侧面的形状，高度，宽度。

#### 截面效果图

![](http://mingchuan.wang/img/Line/Line_1.png)
![](http://mingchuan.wang/img/Line/Line_2.png)
![](http://mingchuan.wang/img/Line/Line_4.png)
![](http://mingchuan.wang/img/Line/Line_5.png)
![](http://mingchuan.wang/img/Line/Line_6.png)

## 原理

![](http://mingchuan.wang/img/Line/Point.png)

raw_face_struct raw_faces[] = 
	{
		{ { 0, 9, 11, 2 } },
		{ { 9, 8, 10, 11 } },
		{ { 8, 1, 3, 10 } },
		{ { 1, 3, 7, 5 } },
        
		{ { 14, 7, 5, 12 } },
        { { 15, 14, 12, 13 } },
		{ { 6, 15, 13, 4 } },
		{ { 4, 6, 2, 0 } },
        
		{ { 0, 4, 13, 9 } },
		{ { 9, 13, 12, 8 } },
        { { 8, 12, 5, 1 } },

		{ { 3, 7, 14, 10 } },
		{ { 10, 14, 15, 11 } },
		{ { 11, 15, 6, 2 } }
	};


## 优化
场景中出现50堵墙，那个生成的顶角线和地脚线的数量将是至少200个（相交的端点出已删除优化），帧数严重下降。必经ProceduralMesh 是UE4的测试的功能，性能远低于StaticMesh。在万能的github上找到了一个大佬的一段代码 RuntimeMeshComponent，同样具备ProceduralMesh的加减乘除功能。链接：https://github.com/Koderz/RuntimeMeshComponent ，替换了ProceduralMeshComponent，帧数有了明显的提升可以再极限情况下的20帧提升到了30帧...UE4这块国外大佬还是牛逼啊！认真研究大佬代码。
