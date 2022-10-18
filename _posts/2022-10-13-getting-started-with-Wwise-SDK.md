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


作为一个技术音频，根据实际项目帮助团队制作各种优化工作流的工具是非常重要的工作内容。本文将以官方文档的示例为引子并结合 3 个 Wwise 小工具，展开说说我的 Wwise 底层学习之路。制作这些工具需要掌握基础的 Python 编程以及对 PySimplyGUI 这个库的使用来完成交互图形界面的搭建，还需要熟悉 Wwise 的基本概念和操作使用，这些部分的内容不会在文中详细展开。

## 工具 1 ：为选中的对象创建 SoundBank

![](/img/Wwise-tool-create-soundbank.png)

在需要为大量 Event 创建不同 SoundBank 的情况下，我们可以通过调用 WAAPI 实现客制化工具来优化这一流程，大大节省冗余繁琐的操作所花费的时间。上图是这个工具的界面，程序会自动识别当前 Wwise 工程中用户在 Event 选项卡下选中的对象，并根据对象的路径以及单选按钮中的限制条件为这些对象创建对应的 SoundBank。

现在来到第一个需要解决的问题：如何让程序获取 Wwise 工程的相关信息，即如何让我们的程序和 Wwise 通信，并调用 WAAPI（Wwise Authoring API）。

### 建立通信

有三种协议可与 Wwise 建立通信，包括：WAMP、 HTTP POST 或是在 Wwise 插件中调用 `AK::Wwise::Plugin::Host::WaapiCall()`。其中 WAMP（The Web Application Messaging Protocol） 协议的性能最佳，且支持**远程过程调用（RPC）**和**发布订阅**功能，因此通常选择这一协议。

官方文档中对 WAMP 是什么作了补充说明：WAMP 旨在连接分布式应用中的应用组件。WAMP 使用 WebSocket 作为其默认传输方式，允许有序、可靠、双向、信息为导向的通信。 **WAMP 允许客户端使用 JSON 参数来调用函数并获取结构性 JSON 结果**。

另外，利用 WAMP 协议调用 API 的可选编程语言有多种，官方推荐的有：C++、C、Python3.6+，我们结合一个官方 Python3.6+ 的示例代码来说明：

1. 在当前 Python 环境下安装如下依赖

	```py
	py -3 -m pip install waapi-client
	```
	
2. 导入依赖，建立通信

	```py
	# 导入 WAAPI 相关依赖
	from waapi import WaapiClient, CannotConnectToWaapiException
	from pprint import pprint
	
	# try except 作为连接失败的异常处理
	try:
		# 使用 URL 连接 WAAPI，这里显式写出了默认地址和端口，可以根据需要在 Wwise 中自行修改端口
		# 此外使用 with as 能保证代码运行至最后自动断开程序和 WAAPI 的连接
		with WaapiClient(url='ws://127.0.0.1:8080/waapi') as client:
      		
      		'''
      			JSON-RPC: JSON-Remote Procedure Call
      		'''
		
	except CannotConnectToWaapiException:
    		print("Could not connect to Waapi: Is Wwise running and Wwise Authoring API enabled?")
	```
	根据上面代码段中的注释我们得以了解通信是如何被建立的，现在终于来到了最后一部分，我们来看看例子中是如何通过 RPC 调用 API 获得要想的信息，实现各种客制化的功能。
	
### 调用 API

上面代码被省去的部分：

```py
	# 获取工程信息，无参数调用
	print("Getting Wwise instance information:")
	result = client.call("ak.wwise.core.getInfo")
	pprint(result)
		    
	# 查询指定对象的信息	，JSON 参数调用	
	print("Query the Default Work Unit information:")
	object_get_args = {
	# JSON 参数段
		 "from": {
	      		"path": ["\\Actor-Mixer Hierarchy\\Default Work Unit"]
		},
	     "options": {
	     		"return": ["id", "name", "type"]
	 	}
	}
	result = client.call("ak.wwise.core.object.get", object_get_args)
	pprint(result)
```
	
程序运行结果：
		
![](/img/wwise-exmp1.png)

首先让我们回到上面曾提到过的两个定义，**远程过程调用**和**发布订阅**，它们也分别对应了通过 WAMP 协议支持的两种通信方式：

* 远程过程调用 - client.call()

	允许针对 Wwise 设计工具远程调用函数，我们可以在程序中调用官方 reference 给出的 Functions 中的所有 API。调用这些 API 后我们可以直接获取几乎所有 Wwise 对象的诸多属性，并对它们做出各种操作，我们的第一个工具就是在此基础上实现的。

* 发布订阅 - client.subscribe()

	允许在 Wwise 设计工具中出现变动时接收通知，我们可以在程序中调用官方 reference 给出的 Topics 中的所有 API。调用这些 API 后我们可以订阅 Wwise 中对应的动作，比如订阅某个对象的名称，当它的名称修改后就能得到返回的具体信息。
	
可以看到代码中我们通过`ak.wwise.core.getInfo`获取了当前 Wwise 工程的全局信息，通过`ak.wwise.core.object.get`查询到了某个指定对象的基本信息，至此我们终于到达了 Wwise SDK 的核心部分 —— [WAAPI](https://www.audiokinetic.com/zh/library/edge/?source=SDK&id=waapi.html)。

> ## 用例示例
> Wwise Authoring API 可以和以下项目集成：
>
>* 游戏引擎
>* 对话管理管线
>* 用于声音设计、编辑、对话录音或音乐制作的 DAW
>* 各种各样的脚本
>
> The Wwise Authoring API 可以用于：
>
>* 任务自动化，如导入音频文件或创建 Wwise 对象
>* 在移动设备上远程控制 Wwise
>* 实现自定义 Wwise 界面
>* 向 Wwise 添加自定义功能。请参阅 定义命令扩展 。
>
> ## 工作原理
> WAAPI 是一种允许其他进程与 Wwise 设计工具进行通信的 API。WAAPI 支持双向通信，允许进程进行远程程序调用并订阅相关主题，以便在 Wwise 中出现变动时及时获悉。
>
> WAAPI 允许访问三个不同层次的功能：
>
>* Wwise 用户界面：视图、选项、命令等
>* Wwise 设计工具核心：工程和对象、SoundBank、音频文件、走带等
>* Wwise 声音引擎：Game Object、Post Event、RTPC Value 等
>
WAAPI 可通过各种编程语言来使用。
	
Wwise 的官方文档强大且全面，但 WAAPI 的部分是按字典序分类排列的，且不存在关联性缺乏整理，对于初学者非常不友好。受到溪夜老师的文章[人人都能用 WAAPI（一）概述](https://xiye.art/2020/09/01/WAAPI%20一文通（一）)启发，我在过文档学习各 API 功能的过程中尝试去使用思维导图对它们进行整理分类。

![]()

如上图所示，按照远程过程调用和发布订阅的差异，官方文档将 WAAPI 分为 Fuctions 和 Topics 两大类，再按照类的命名我们还可以对所有的 API 进一步细分。图中的各个绿色虚线框是针对每个 API 的实际功能进行的更细致的分类。除了上述的分类方法，我们还可以按有无输入参数，有无返回值来分类。应用范围较广的 API 都有着对应的官方示例，利用好这些示例可以帮助我们快速熟悉这些 API。对于没有示例的 API，除了查看官方文档中对其功能简单的描述外，还要仔细查看它的参数以及返回值，自行判断它的应用场景。

再次回到官方示例中使用到的两个 API，






# 文章正在施工中！！
