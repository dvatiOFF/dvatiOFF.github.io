---
layout: post
title: "暗区突围 - 战术耳机&防爆头盔音频模块的复现与思考"
subtitle: "Arena Breakout - Reproduction of audio modules for tactical earphones and explosion-proof helmets"
author: "OFF"
header-style: text
tags:
  - Wwise
  - Unity
  - Audio Modules
  - Reproduction
---

我在暑期实习时对两款竞品射击类手游的音频模块进行了对比分析，并针对暗区突围中独有的模块进行了复现，这篇文章是复现的思路和总结。

## 音频模块分析

战术耳机和防暴头盔是暗区突围游戏中仅有的在玩家穿戴后影响听觉效果的物品(这两类物品不能同时佩戴)，这个模块也是萤火突击中不具备的，具体功能如下：

* 防暴头盔-**声音阻隔**: 降低除了界面音效之外的所有声音（包括环境声、枪声、自己的脚步声、他人脚步声等）的响度，并增加一个低通的效果，使声音听起来更沉闷真实

* 战术耳机-**降噪**: 降低玩家听到的环境声音量（不包括场景中的 3D 环境音效如风车声等）

* 战术耳机-**拾音**: 暗区突围中对声音的处理不分敌我，友军或敌军的枪声、脚步声等的混响效果完全一致。拾音提升了玩家对地图中大部分关键音效的最大衰减距离，即能帮助玩家听到更远的关键声音信息。增益的作用对象有地图探索声（玻璃破碎声、开关门、搜包等）、其他角色声（脚步声、AI触发的语音、使用物品声、近战武器声、枪声、手雷爆炸声等），但不增益任何环境声

![](https://github.com/dvatiOFF/dvatiOFF.github.io/blob/master/img/headphone-helmet.png?raw=true)

## 复现思路

选择在同样是 3D 游戏的 Wwise 的官方示例 Wwise Adventure Game(WAG) 中进行复现，WAG中的总线划分较为简单，下面复现会涉及到的有 Ambient Bus（环境总线）、Enemies Bus（敌人总线）、Player Bus（玩家总线）、AUX Bus(涉及空间混响的辅助总线)，具体功能的复现思路如下：

* 防暴头盔-**声音阻隔**: 设置 Event，调整上述提到的前三类总线的音量和 LP 的数值，把 AUX Bus 的音量输出调到极低（阻隔混响声）;对于玩家在做出攻击动作或受到伤害时自身发出的音效无需调整保留干音即可，再将这部分音效输出到一个新的 Bus 上就能达到更真实的效果

* 战术耳机-**降噪**: 设置 Event，调整 Ambient Bus 的音量

* 战术耳机-**拾音**: <del>设置 Event，调整 Enemies Bus 的音量 ; 切换总线对象和音频对象的衰减</del>

## 问题和分析

最初通过发送 Event 修改 Bus 音量的复现并不符合暗区突围中的效果，因为拾音的设定是让玩家对关键声音听得更远而不是更响，之后意识到需要做的是在游戏运行时修改对应音频对象的衰减曲线 `Max Distance` 的值。又尝试了设置两种不同的衰减曲线，并通过 State 中的 `enable attenuation` 属性来切换音频及其对应总线的衰减来达到想要到效果，但也失败了，原因是衰减的计算需要 Listener 和 Emitter 之间的空间信息，但总线对象默认绑定在 Listener 上，所以计算总线衰减的 Distance 实际一直为 0，音量不会随距离发生变化。

## 解决方法

1. 设置 Switch 容器: 在 ShareSets 中设置两套衰减，复制每一个相关的音频对象，应用不同的衰减曲线后放入 Switch 容器中，根据 State 的不同切换播放对应的音频

2. 调用 SDK 函数: 在学习 WAAPI 的过程中，发现 soundengine 部分的 `setScalingFactor` API 的功能正好是控制游戏对象的缩放系数，依此来修改衰减的计算结果。因此在Unity脚本中直接调用对应的函数 `AkSoundEngine.SetScalingFactor()` 可达到控制衰减范围的效果，Unity 脚本实现如下：

```ts
void EnableTacticalHeadset()
{
    // Double the gameobject's sound pickup range
    AkSoundEngine.SetScalingFactor(gameObject, 2.0f); 
}
```

效果对比: 内存占用方面，由于**方法 1** 复制了一份同样的音频应用了不同的衰减，从 SoundBanks 的生成情况来看，相比**方法 2** 会额外占用大约 14% 的内存（3325.39KB → 3793.65KB，单个 Enemy GameObject 涉及的音频），在内存资源紧张的情况下要谨慎使用。从工作量来看，**方法 1** 需要给每一个相关的音频资源分配 Switch 容器、设置衰减、修改相关 Event 的引用对象，工作量比较大；**方法 2** 则只需要编写新的脚本引用 SDK 函数设定好所需的变化系数并应用至引擎中相关的预设体中即可。对于拾音功能的复现，两种方法的最终实现效果一致，**方法 1** 直接操作了音频对象，粒度更细，更适合需要精细制作的情况；而**方法 2** 是直接修改了相关游戏对象的衰减属性，更方便快捷。

## 复现展示

<iframe src="//player.bilibili.com/player.html?aid=303425834&bvid=BV1NP411n79H&cid=845139297&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="700" height="524"> </iframe>

