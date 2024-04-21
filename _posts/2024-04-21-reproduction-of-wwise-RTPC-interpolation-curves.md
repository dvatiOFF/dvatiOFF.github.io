---
layout: post
title: "Wwise RTPC 插值曲线复现"
subtitle: "Reproduction of Wwise RTPC Interpolation Curves"
author: "OFF"
header-img:   "img/RTPCCurve-bg.png"
header-mask:  0.3
tags:
  - Wwise
  - Interpolation Curves
---

本文（水一期） Wwise RTPC 插值曲线的复现，契机是项目移植受限，需要手搓部分 Wwise 的特性。在没有源码的参考的情况下想要完全 1:1 复现 Wwise 中的十种曲线，虽然涉及的知识仅限初中数学难度，但做的时候发现这更像一种费时的体力活...


Wwise RTPC 的 Curve 属性包含了十种类型的插值曲线，这十种曲线在 Authoring 中、wwu 文件内、以及官方文档中的显示名称都不一致（一致性也太差了吧！）。

想要获取项目中各个对象使用的 RTPC 的插值曲线类型需要绕很多弯路，因为不能通过 WAAPI 直接 get 到曲线类型，这部分信息只能直接去相关 wwu 中找到对应的 Curve.PointList.Point.SegmentShape 来获取，手动测试获取后发现返回值是 string 类型的曲线名称；然而 Action 对象的 `FadeInCurve` / `FadeOutCurve` 属性包含其中九种曲线，[官方文档](https://www.audiokinetic.com/en/library/edge/?source=SDK&id=wwiseobject_action.html)给出的数据类型是 int 类型及对应的值.


| 	Authoring   | 官方文档 | wwu   |
|:--------:|:-------:|:--------:|
| Logarithmic (Base 3)" | 0 - Logarithmic (Base 3)" | Log3 |
| Sine (Contant Power Fade In) | 1 - Sine | Log2 |
| Logarithmic (Base 1.41) | 2 - Logarithmic (Base 1.41) | Log1 |
| Inverted S-Curve | 3 - Inverted S-Curve | InvertedSCurve |
| Linear | 4 - Linear | Linear |
| S-Curve | 5 - S-Curve | SCurve |
| Exponential (Base 1.41) | 6 - Exponential (Base 1.41) | Exp1 |
| Reciprocal Sine | 7 - Reciprocal Sine | Exp2 |
| Exponential (Base 3) | 8 - Exponential (Base 3) | Exp3 |
| Constant |  | Constant |


 

## Constant
