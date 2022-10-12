---
layout: post
title: "荒野大镖客2 环境声分析"
subtitle: "Red Dead Redemption 2 - Ambient Sounds Analysis"
author: "OFF"
header-img:   "img/RDR2-bg.jpg"
header-mask:  0.3
tags:
  - AAA Games
  - Ambient Sounds
  - Audio Modules Analysis
---

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

 从画面、声音到故事背景和游戏机制，荒野大镖客2 可能是目前人类所开发的所有游戏中真实度最高、沉浸感最强的一作。得益于R星在近二十年一项又一项跨世代大作的开发过程中迭代积累的经验技术，他们的音频团队在业内也堪称顶级。
 
 荒野大镖客2 暴露出的音频接口就有大约 600 个，对这样一个庞然大物的所有音频模块进行详尽的分析太过艰巨，本文尝试从感知性、功能性和历史文化象征三个角度展开，用一些典型的例子结合录屏对其中涉及的环境声进行分析。
 
 荒野大镖客2 中环境相关的[音频接口](https://mp.weixin.qq.com/s/4GiLi4FwfRxdg5p6HEmDaw)：
 ![](/img/RDR2-audio-interface.png)

## 感知性

每个人都有基于真实世界习得的关于各种声音的听感经验，在荒野大镖客2这样画面真实性极高的开放世界游戏中，玩家的耳朵会对接收到的一切声音更加严苛，被动地感受并评估听到的一切是否合理。总的来说，根据玩家所处的具体场景，声音种类的丰富合理，声音效果的真实可信构成了环境声的感知性。

### 生物
荒野大镖客2动物系统包含多达 177 种从哺乳类、鸟类到两栖类爬行类等不同的物种，观察记录和捕猎系统也极大提升了游戏的可玩性和丰富度。其中丰富的鸟类叫声也构成了非城市区域环境声中最重要的部分。游戏根据玩家所在区域，会在场景中随机播放该区域包含的 3D 鸟类叫声音频。	
![](/img/RDR2-animals.jpg)

* 地区
	
	根据游戏内的地区列举了该区域环境声中常出现的物种：
	
	* 森林：雪松太平鸟、东部野火鸡、蓝松鸦

	* 水域：双冠鸬鹚、太平洋潜鸟、黄嘴潜鸟、褐鹈鹕

	* 牧场：蓝斑浣熊猎犬、鸡、中国大猪、美利奴绵羊
	
	* 湿地：绿头鸭、北美西部蟾蜍、美国牛蛙、美洲鹤
	
	* 山部：白头雕、巴尔的摩黄鹂、金雕
	
	* 草原：环颈雉、加拿大大雁、亚欧树雀鹀
	
	* 沼泽地：雪鹭、沙丘鹤、棕颈鹭
	
	* 城市：美国家猫、环嘴鸥、寻回犬
	
	* 干旱地区：沙漠盘羊、美国獾、索诺拉沙漠蟾蜍
	
	* 常见：北美西部渡鸦、西部火鸡秃鹫、美洲乌鸦、海角鸮


	
* 时间

	根据各物种真实的作息，每天的不同时间段的环境声中出现的生物音效也不同，如夜晚会播放狼嚎、猫头鹰叫声等。由于环境声中有大量的鸟类叫声，根据鸟类的作息，在清晨和午后等不同时段也会有不同的鸟叫种类和层次。
	
	* 夜行：美洲树雀鹀、金冠雀、西部渡鸦、美国灰狐、加州峡谷郊狼、红腹啄木鸟等
	
	* 清晨
		
		鸟类最活跃的时间，此时环境声中鸟类叫声占比更多丰富度更高。
		
		<div class="iframe-container">
			&nbsp&nbsp&nbsp&nbsp<iframe src="//player.bilibili.com/player.html?aid=388987775&bvid=BV1Xd4y1i7Nh&cid=856375481&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
		</div>

	* 午后

		鸟类叫声更平静，种类也不如清晨丰富。

		<div class="iframe-container">
			&nbsp&nbsp&nbsp&nbsp<iframe src="//player.bilibili.com/player.html?aid=388949530&bvid=BV1rd4y1i7H6&cid=856377333&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
		</div>
		

### 天气

荒野大镖客2 丰富的天气变化系统将游戏体验的真实度提升到了更高的层次。

* 雨

	下雨导致地面变得泥泞进而影响地图内的脚步声交互音效。
	
* 雪

	通过风声、踩雪声影响玩家的感知（下雪本身是无声的）。

* 阵风
	
	淡入淡出：强风的音效从起风至最强需大约 30 秒，期间禽类叫声完全停止，风停的淡出也需大约 30 秒，淡出过程中禽类叫声开始逐渐恢复。
	
	强风音效分层：1. 风噪（表现呼啸的两个中高频层；表现风力的低频层）2. 枝叶声（高层枝叶的较柔和的沙沙声；中低层枝叶表现风力的簌簌落落声）这里枝叶层的声音可能随实际树木的不同有区分，比如阔叶树木和杉木，杉木就以柔和的沙沙声为主。
	
	空旷地带：风噪声大，也有可辨的远处的枝叶声。
	
	由空旷地带移动至树木附近：枝叶声渐强，最终枝叶声更突出。
	
	<div class="iframe-container">
		<iframe src="//player.bilibili.com/player.html?aid=261482077&bvid=BV1Qe411575E&cid=856370888&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
	</div>

* 打雷

	雷暴本身的音效根据动画以及距离持续大约2-3秒，后续的巨大混响相应持续15-20秒
对其他环境声的影响：雷暴音效响起的那几秒会屏蔽绝大多数原本的环境声（如鸟叫声、马车声，路人谈话声等；但仍能听到教堂钟声、火车站点铃声等优先级很高的音效），环境声在混响开始大约5-8秒后逐渐恢复。
	
	<div class="iframe-container">
		<iframe src="//player.bilibili.com/player.html?aid=773875748&bvid=BV1Z14y1j7w3&cid=856369752&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
	</div>

### 交通工具
* 火车

	火车驶近的过程中还伴随巨大的鸣笛，一般在第一声鸣笛后，环境声中的鸟虫类叫声会立刻停止，至火车进站再启动驶远后这些叫声才逐渐恢复。
	
	<div class="iframe-container">
		<iframe src="//player.bilibili.com/player.html?aid=986463266&bvid=BV1s44y1f7Xi&cid=856369924&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
	</div>

### 城市
通过设定划分复杂的区域以及制定精妙的播放规则，R-Star 为玩家呈现了极具沉浸感的 20 世纪初美洲中西部城市声景。

* 构成

	圣丹尼斯城中的声景由各类复杂的点声源构成，大多音效都播放在玩家视线之外，播放的位置和内容随机性极高，几乎没有重复内容，比如城市中可以听到大量日常物品类的声音，营造了真实的生活气息。复杂精妙的播放规则，根据空间和距离的动态混音让玩家在游戏中闭上眼光用耳朵听就能感受到这个鲜活的城市的每个细节。其中教堂钟声是优先度最高的音效，且整个城市都可以听到。包括酒馆钢琴在内的乐器演奏声的优先度也很高，音乐声的覆盖范围很大。

	* 白天街边
	
		1. 视野内
			
			玩家视野内的对象发出的声音只占整个声景极小一部分，而且很多情况下视野内的对象并不发出声音，几乎不影响整体构成，但玩家视野内发出的声音优先度都很高。
			
			**路人相关**：对话声、扫地声、脚步声
			
			**马匹相关**：马车减速刹车声、马啸、挥鞭抽马声、马蹄声、马车车轮声、马车行驶中的高频吱嘎声、铃铛声
			
		2. 视野外

			**教堂钟声**
			
			**酒馆钢琴声**
			
			**物品相关**：多种不同的开关门声、使用餐具声、金属碰撞声、电话铃声、拍打衣物声、木头嘎吱声、敲打瓷器声等
			
			**生物相关**：狗叫声、鸭子叫声、鸥类叫声
			
			**路人相关**：对话声、大笑声、咳嗽声
			
		3. 底噪
		
			**微风**

		
	<div class="iframe-container">
		<iframe src="//player.bilibili.com/player.html?aid=303928274&bvid=BV1bP41177pR&cid=856377947&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
	</div>
	
	* 夜晚楼顶

		1. 视野外
			
			**路人相关**：对话声、扫地声、脚步声
			
			**马匹相关**：马车减速刹车声、马啸、挥鞭抽马声、马蹄声、马车车轮声、马车行驶中的高频吱嘎声

			**教堂钟声**
			
			**酒馆钢琴声**
			
			**物品相关**：多种不同的开关门声、瓶罐掉落声、金属碰撞声、电话铃声、玻璃破碎声、木头断裂声等
			
			**生物相关**：多种狗叫声、多种虫鸣声
			
			**路人相关**：一两处突出的对话声（叫骂、吵架、打招呼、大笑）、口哨声、几处模糊难辨的对话声
			
		2. 底噪
		
			**微风**

	<div class="iframe-container">
		<iframe src="//player.bilibili.com/player.html?aid=603881382&bvid=BV1YB4y1j7xi&cid=856378362&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
	</div>

* 空间切换

	游戏对整个城市的空间进行了细致的划分并做了对应的混音和播放规则，这里列举了一些典型的例子。
	
	* 建筑室内外
		
		游戏中连通建筑内外的装置只有门，因此门相当于一个开关，玩家通过与门的互动达成了混音的切换。
		当玩家在封闭的室内（关门时），室外的大多数环境音效会被屏蔽，但能听到马车声和微弱的鸟类叫声。
		当玩家开门后，所有环境音效恢复并在大约 0.3 秒内响度渐强至最大。
		当门在打开时，根据玩家相对门的角度和距离，会对室外的环境音效的响度和声相做对应的处理，出门后也没有打开门之后所有声音一瞬间涌入的感觉。

		<div class="iframe-container">
			&nbsp&nbsp&nbsp&nbsp<iframe src="//player.bilibili.com/player.html?aid=773925780&bvid=BV1p14y177YU&cid=856371071&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
		</div>
	
		
	* 巷子深处
			
		巷子深处的环境更安静，在这里听到的所有巷外音效整体响度降低，也能听到更多外部听不到的生物音效，比如虫鸣、各类鸟叫、猫叫等。
		
		<div class="iframe-container">
			&nbsp&nbsp&nbsp&nbsp<iframe src="//player.bilibili.com/player.html?aid=858890564&bvid=BV13V4y1L7e7&cid=856380434&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
			</div>


	* 进出巷子

		和开着门情况下的室内外变化规则类似，但响度和声响的渐变更柔和。
		
		<div class="iframe-container">
			&nbsp&nbsp&nbsp&nbsp<iframe src="//player.bilibili.com/player.html?aid=561381508&bvid=BV17e4y1q7fc&cid=856379367&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
			</div>

## 功能性

或是为了引导、提示玩家的行为，又或是为了推动剧情的发展，游戏中的这些声音在恰到好处的时机如灵光突然出现，让玩家仅仅通过耳朵就能直接感受到游戏设计者的意图和巧思。这些不只是声音的声音体现了它们强大的功能性。

### 钢琴声
在城市嘈杂环境声中，却能听到圣丹尼斯旅馆传来的清晰明亮的钢琴声，借助这里特殊的混音处理，可以吸引玩家循着声音前往探索，解锁进一步的剧情。

### 腐食鸟类的叫声
像秃鹫、乌鸦一类具有辨识度的鸟类叫声在地面近处触发是，能够提示玩家附近有动物或者人的尸体。

### 教堂钟声
从功能性上来说，圣丹尼斯的教堂在每个整点会敲钟报时，在早中晚的特殊时分还会敲长钟精确报时，提醒玩家当前的时间。

### 暴风雪声
初章因剧情一直处于雪天之中，当玩家可自由探索时离开营地超出一定距离后，会触发暴风雪天气，此时因恶劣天气玩家的血量会持续降低，软强制玩家回到划定区域内。与上文提到的阵风音效不同，触发的暴风雪声高频的呼啸更突出，更符合暴风雪声的听感。


<div class="iframe-container">
	<iframe src="//player.bilibili.com/player.html?aid=901388220&bvid=BV1CN4y1A7c8&cid=856365452&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" high_quality="1" allowfullscreen="true"> </iframe>
</div>


## 历史文化象征

人不能脱离经验去创造，而经验来自于历史。虽然人物角色源自虚构，但荒野大镖客2 整个的游戏背景，一切在游戏中的所闻所见，无不指向了十九世纪末二十世纪初美国中西部的那段历史。玩家行走的这片土地上，有无数细节映射了那个时代的历史与文化，通过这些反应了历史与文化的真实声音的填充，整个西部世界的一切也变得更加丰满。

### 语言
各地区居民的语言交流是游戏声景的一部分，在圣丹尼斯中行走，经常能听到周围的NPC用各种语言交流

* 法语

	圣丹尼斯的原型应该是法国在密西西比河入海口建立的殖民城市新奥尔良，因此法国人也是圣丹尼斯最多的非英语使用人群

* 德语

	十九世纪末中西部的新德国移民几乎不会说英语
* 广东话

	华人劳工（主要来自广东台山、开平、新会和恩平）远渡重洋在长达大半个世纪的时间里作为苦力铺设纵横北美大陆的太平洋铁路的历史背景，能够进入城镇安家的华人是极少数的幸运者。圣丹尼斯的原型新奥尔良城历史上也曾有过中国城。
* 波兰语
* 美洲印第安语
* 印式英语
* 英式英语

### 建筑
* 宗教教堂

	圣丹尼斯是法国人建立的城市，当地的宗教自然是天主教，游戏剧情中也涉及了很多关于天主教和清教的矛盾冲突。
圣丹尼斯教堂的钟声几乎能覆盖整个城市，教堂钟声也是游戏中城市环境声构成中优先度最高的声音，即使在封闭的室内也能听到。

### 生物
在丰富的生物系统中也能找到很多物种与游戏文化历史背景的联系

* 美利奴绵羊

	16世纪中叶，西班牙美利奴羊传入美国
	
* 美洲野牛
	
	美洲野牛是印第安人赖以生存的重要资源，对美洲野牛的屠杀也是对美洲印第安原住民迫害的剪影

### 音乐
* 古典音乐

	游戏背景的时间基本是在1899年及以后，游戏的留声机中播放的音乐都颇有考究有据可循，比如在游戏初期在 Valentine 附近的营地中曾播放过意大利作曲家 Giuseppe Verdi 于1853年完成的 Libiamo (Drinking Song) - 茶花女（饮酒歌），不过游戏中播放的版本显然是现代乐团录制的。
