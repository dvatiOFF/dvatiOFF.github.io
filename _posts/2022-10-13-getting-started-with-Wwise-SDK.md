---
layout: post
title: "Wwise SDK 入门 - 以几个小工具为例"
subtitle: "Getting Started With Wwise SDK"
author: "OFF"
header-img:   "img/wwise-bg.jpg"
header-mask:  0.3
tags:
  - Wwise
  - Python
  - WAAPI
---


作为一个技术音频，根据实际项目帮助团队制作各种优化工作流的工具是非常重要的工作内容。本文将以 3 个 Wwise 相关的小工具为例子，展开说说我的 Wwise 底层学习之路。制作这些工具需要掌握基础的 Python 编程以及对 PySimplyGUI 这个库的使用来完成交互图形界面的搭建，还需要熟悉 Wwise 的基本概念和操作使用，这些部分的内容不会在文中详细展开。

## 工具 1 ：为选中的对象创建 SoundBank

![](/img/Wwise-tool-create-soundbank.png)

在需要为大量 Event 创建不同 SoundBank 的情况下，我们可以通过调用 WAAPI 制作工具来优化这一流程，大大节省冗余繁琐的操作所花费的时间。上图是这个工具的界面，程序会自动识别当前 Wwise 工程中用户在 Event 选项卡下选中的对象，并根据下方的单选按钮为这些对象创建对应的 SoundBank。

问题来到了第一步：如何让程序获取 Wwise 工程的相关信息，即如何让我们程序和 Wwise 通信。

### 建立通信




# 文章正在施工中！！
