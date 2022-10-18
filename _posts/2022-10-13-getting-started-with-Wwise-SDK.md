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

在需要为大量 Event 创建不同 SoundBank 的情况下，我们可以通过调用 WAAPI 制作工具来优化这一流程，大大节省冗余繁琐的操作所花费的时间。上图是这个工具的界面，程序会自动识别当前 Wwise 工程中用户在 Event 选项卡下选中的对象，并根据对象的路径以及单选按钮中的限制条件为这些对象创建对应的 SoundBank。

问题来到了第一步：如何让程序获取 Wwise 工程的相关信息，即如何让我们的程序和 Wwise 通信，访问 WAAPI。

### 建立通信

有三种协议可与 Wwise 建立通信，包括：WAMP、 HTTP POST 或是在 Wwise 插件中调用 `AK::Wwise::Plugin::Host::WaapiCall()`。其中 WAMP 协议的性能最佳，且支持**远程程序调用**和**发布订阅**功能，因此通常选择这一协议。

官方文档中对 WAMP 是什么作了补充说明：WAMP 旨在连接分布式应用中的应用组件。WAMP 使用 WebSocket 作为其默认传输方式，允许有序、可靠、双向、信息为导向的通信。 **WAMP 允许客户端使用 JSON 参数来调用函数并获取结构性 JSON 结果**。

目前我们无需深究 WAMP 之下到底是什么，对于这段定义我们只需关注加粗的这部分，之后再回头来看看它到底意味着什么。

另外，利用 WAMP 协议调用 API 的可选编程语言有多种，官方推荐的有：C++、C、Python3.6+，我们先结合一个官方 Python3.6+ 的示例代码说明建立通信的过程：

1. 在当前 Python 环境下安装如下依赖
‘
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
      		
	      		# 获取工程信息
			print("Getting Wwise instance information:")
			result = client.call("ak.wwise.core.getInfo")
			pprint(result)
			    
			# 查询指定对象的信息			
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
      		
	except CannotConnectToWaapiException:
    		print("Could not connect to Waapi: Is Wwise running and Wwise Authoring API enabled?")
	```
![](/img/wwise-exmp1.png)

根据上面代码段中的注释我们了解了整个通信是如何被建立的。当通信建立后就可以访问所需要的 API 了，比如通过`ak.wwise.core.getInfo`获取当前 Wwise 工程的全局信息，通过`ak.wwise.core.object.get`查询某个指定对象的基本信息，至此我们终于到达了 Wwise SDK 的核心部分 —— WAAPI。

首先让我们回到上面曾提到过的两个定义，**远程程序调用**和**发布订阅**，它们也分别对应了 WAAPI 的两种：

* 远程程序调用 - Fuctions

	允许针对 Wwise 设计工具远程调用函数，我们可以在程序中调用官方 reference 给出的 Functions 中的所有 API。调用这些 API 后我们可以直接获取几乎所有 Wwise 对象的诸多属性，并对它们做出各种操作，我们的第一个工具就是在此基础上实现的。

* 发布订阅 - Topics

	允许在 Wwise 设计工具中出现变动时接收通知，我们可以在程序中调用官方 reference 给出的 Topics 中的所有 API。调用这些 API 后我们可以订阅 Wwise 中对应的动作，比如订阅某个对象的名称，当它的名称修改后就能得到返回的具体信息。
	
上面示例代码中的文字






# 文章正在施工中！！
