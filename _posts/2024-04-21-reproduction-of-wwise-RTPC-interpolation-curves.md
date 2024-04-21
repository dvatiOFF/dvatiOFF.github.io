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


Wwise RTPC 的 Curve 属性包含了十种类型的插值曲线，这十种曲线在 Authoring 中、wwu 文件内、以及官方文档中的显示名称都自成一派（幽默一致性）。

首先通过 WAAPI ak.wwise.core.object.get 获取到 RTPC 的 Curve 曲线类型，手动测试获取后发现返回值是 string 类型的曲线名称；然而 Action 对象的 `FadeInCurve` / `FadeOutCurve` 属性包含其中九种曲线，[官方文档](https://www.audiokinetic.com/en/library/edge/?source=SDK&id=wwiseobject_action.html)给出的数据类型是 int 类型及对应的值，三类显示如下：


| 	Authoring   | 官方文档 | WAAPI   |
|:--------|:-------|:--------|
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


## Input
从 WAAPI ak.wwise.core.object.get 返回的曲线信息如下：
```JSON
"Curve": {
	        "id": "{261609FF-8104-4BB5-AB07-160DF0A8B534}",
	        "points": [
	            {
	                "x": 0.0,
	                "y": 11.64929,
	                "shape": "Log1"
	            },
	            {
	                "x": 3.14717,
	                "y": 7.6169,
	                "shape": "Linear"
	            },
	            {
	                "x": 4.36981,
	                "y": 0.67505,
	                "shape": "Exp2"
	            },
	            {
	                "x": 24.0,
	                "y": 11.649,
	                "shape": "SCurve"
	            }
	        ],
	        "@Flags": 3
	    },
```
各个点的 `shape` 属性表示该点和下一相邻点之间的插值曲线类型（最后一个点忽略），因此要根据 x （GameParameter 的值）计算 y (对应属性值)，需要先判断 x 位于哪两个点之间，再将两点坐标信息及 x 的值代入下面的函数中计算 y 的值。


## Log & Exp
在 [GeoGebra](https://www.geogebra.org/graphing) 中画了半天各种对数和指数函数，最终怎么都无法和 Authoring 中的曲线图拟合，最后崩溃发现它们的真面目竟然都是**幂函数**...

总之经过反复试错，这两大类共 4 种曲线（Log3,Log1,Exp3,Exp1），是由幂函数在**限定值域**下通过**横纵放缩**及**平移、翻转变换**得来的，曲线图如下：


### Exp3
基函数: y = x^3 x ∈ [0, t]
根据起止点的坐标对基函数进行放缩和翻转得到。
```csharp
public static float Exp3(float x1, float y1, float x2, float y2, float x)
{
    // Exponential (Base 3)
    float abs_x = Mathf.Abs(x2 - x1);
    float abs_y = Mathf.Abs(y2 - y1);
    float scale = Mathf.Pow(abs_x, 3) / abs_y;
    
    if (y2 - y1 < 0)
    {
        return -Mathf.Pow((x - x1), 3) / scale;
    }
    else
    {
        return Mathf.Pow((x - x1), 3) / scale;
    }
}
```
### Exp1
基函数: y = x^1.41 x ∈ [-t, 0]
根据起止点的坐标对基函数进行放缩和翻转得到。
```csharp
public static float Exp1(float x1, float y1, float x2, float y2, float x)
{
    // Exponential (Base 1.41)
    float abs_x = Mathf.Abs(x2 - x1);
    float abs_y = Mathf.Abs(y2 - y1);
    float scale = Mathf.Pow(abs_x, 1.41f) / abs_y;
    if (y2 - y1 < 0)
    {
        return -Mathf.Pow(Mathf.Abs(x - x1), 1.41f) / scale;
    }
    else
    {
        return Mathf.Pow(Mathf.Abs(x - x1), 1.41f) / scale;
    }
}
```
### Log3
基函数: y = x^1/3 x ∈ [0, t] (Exp3 的反函数)
根据起止点的坐标对基函数进行放缩和翻转得到。
```csharp
public static float Log3(float x1, float y1, float x2, float y2, float x)
{
    // # Logarithmic(Base 3)
    float delta_x = x2 - x1;
    float delta_y = y2 - y1;
    float abs_x = Mathf.Abs(delta_x);
    float abs_y = Mathf.Abs(delta_y);
    float scale = Mathf.Pow(abs_x, 3) / abs_y; 

    float adjustedX = x - x1 - delta_x;

    if (delta_y < 0)
    {
        return -Mathf.Pow(adjustedX, 3) / scale + delta_y;
    }
    else
    {
        return Mathf.Pow(adjustedX, 3) / scale + delta_y;
    }
}
```
### Log1
基函数: y = x^1/3 x ∈ [0, t] (Exp1.41 的反函数)
由于该函数定义域不能小于 0，手动定义一函数使其定义域和值域与基函数相反，再根据起止点的坐标对基函数进行放缩和翻转得到。
```csharp
public static float Log1(float x1, float y1, float x2, float y2, float x)
{
    // Logarithmic(Base 1.41)
    float delta_x = x2 - x1;
    float delta_y = y2 - y1;
    float abs_x = Mathf.Abs(delta_x);
    float abs_y = Mathf.Abs(delta_y);
    float scale = Mathf.Pow(abs_x, 1.41f) / abs_y;

    float adjustedX = x - x1 - delta_x;
    float absAdjustedX = Mathf.Abs(adjustedX);
    float result = Mathf.Pow(absAdjustedX, 1.41f) / scale;

    if (delta_y < 0)
    {
        if (adjustedX < 0)
        {
            return result + (y2 - y1);
        }
        else
        {
            return -result + (y2 - y1);
        }
    }
    else
    {
        if (adjustedX < 0)
        {
            return -result + (y2 - y1);
        }
        else
        {
            return result + (y2 - y1);
        }
    }
}
```

## SCurve
