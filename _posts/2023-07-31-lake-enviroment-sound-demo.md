---
layout: post
title: "湖泊环境声快速实现"
subtitle: "Rapid implementation of lake environmental sound"
author: "OFF"
header-img:   "img/LakeEnviro-bg.png"
header-mask:  0.3
tags:
  - Wwise
  - Unreal Engine
  - Game Audio
---

本文介绍如何使用 Wwise 在 UE 中搭建一个湖泊环境声的 demo 。

## 水声

对于湖泊环境声来说最重要的莫过于逐渐靠近距离湖岸过程中渐响的水声，由于湖泊是不规则的形状，需要保证玩家在湖岸线的任何一处所听到的响度都基本一致，而进入湖中后水声响度保持不变。

### 水声音效处理

首先在 Wwise 中构建环境声混音相关层级。对于湖泊水声，勾选 Loop 使其循环播放，并对源音频进行处理使其起止响度保持基本一致使重播处听感无异样。在 Positioning 选卡项下开启 3D 空间化（位置+朝向），声相变化选择平衡淡变，调整两者的混音占比使最终玩家在旋转视角时听感更自然，根据后续游戏引擎中实测设置合理的衰减曲线和低通滤波。这里的 Event 只需 Play 该音效即可。

![](/img/LakeEnviro-1-1.png)

### 衰减处理

为使角色在不规则形状的湖泊周围或湖中听到的水声响度基本一致，且随距离有自然的衰减，这里使用了 SDK 中的 `MultiPositionType_MultiDirections` 方法为单个 GameObject （即湖泊）设置多个位置。

如下图所示，首先在湖泊组件下设置多个均匀覆盖表面的点，在这些点的位置播放具有相同衰减的点声源来达到模拟面声源的效果。多个点声源的重叠部分的响度不会叠加而是保持在单个声源的最大值。这种方法设置的点数越多湖泊水声整体的听感越真实，但会消耗更多的CPU来计算音量。

![](/img/LakeEnviro-1-2.png)

具体蓝图实现如下，首先获取上述设置的各点的世界坐标位置存入 TArrary 中，为后续 `Set Multiple Positions` 函数提供位置信息，然后用 `Post Associated Ak Event` 调用该事件即可。

![](/img/LakeEnviro-1-3.png)

### 听者重载

此外还对 Listener 的位置和旋转进行了重载（位于角色处而随 camera 旋转），主要为了解决当 Listener 位于摄像机处，玩家视角旋转导致衰减距离变化使响度也发生变化造成违和感。

![](/img/LakeEnviro-1-4.png)

## 生物音效

而后我们在水声之上加入一层含随机性的生物音效，实际上这里还可以针对实际场景设置细粒度更高的播放规则提高体验。

### 随机触发

利用上述设置的各点声源位置，为湖泊环境声设置随机触发播放的生物音效使整体听感更丰富逼真。在 Wwise 中为每个生物音效添加音高等随机化，放入 Random Container 中用于随机播放，同样地设置合理的 3D 空间化参数、衰减曲线，还可以设置 3D 发声路径使音效具有动态多变的播放轨迹。这里还调整了相关 Event 的触发概率使随机性更强。

![](/img/LakeEnviro-2-1.png)

具体蓝图实现如下，设置自定义随机间隔触发的播放事件，首先随机取出保存在 TArrary 中的 4 个位置作为生物音效的播放位置，同样用 `Set Multiple Positions` 函数设置这些位置，然后用 `Post Associated Ak Event` 调用该事件即可。

## 效果展示

<style type='text/css'>
.iframe-container {
  padding-top: 56.25%;
  position: relative;
}
.iframe-container iframe {
   position: absolute;
   height: 100%;
   width: 100%;
   top: 0;
   left: 0;
}
</style>

<div class="iframe-container">
  <iframe src="//player.bilibili.com/player.html?aid=231653489&bvid=BV1q8411278S&cid=1217889048&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" high_quality="1" allowfullscreen="true"> </iframe>
</div>

## 均匀点分布生成-可能的优化

根据模型人工设置较为均匀散布的点只适用于较小的场景，如果面临的是大型游戏中诸多场景下不同的河流或湖泊，此时需要能自动高效地去生成均匀分布于水体中的点。

这里可以将这些水体模型看作是不规则的二维平面，我们的目标是在不规则二维平面中生成较为均匀分布的点（后续可以人工进行精调使效果更好）。

### 边界获取

可以利用游戏引擎API的自行实现一个边界检测算法，由于将水体模型近似为二维平面，这部分的内容较容易实现。

### 点生成算法

可能的方法有泊松碟采样（Poisson Disk Sampling），或三角剖分算法（Delaunay）
