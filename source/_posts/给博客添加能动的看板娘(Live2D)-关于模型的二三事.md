---
title: 给博客添加能动的看板娘(Live2D)-关于模型的二三事
date: 2017-11-18 04:51:00
categories: lab
tags: [浮动小人,Live2D,看板娘]
urlname: add-dynamic-poster-girl-with-live2d-to-your-blog-01
---
![药水制作师](https://img.imjad.cn/images/2017/11/18/Potion-Maker.png)
## 前言
在两年前差不多这个时候，我写了[一篇文章](https://imjad.cn/archives/code/add-a-floating-villain-to-the-blog)，记载了关于在博客里添加浮动小人的过程。后来因为换了新主题，之前的浮动小人与新主题风格不搭，就去掉了。
而两年后，我注意到[Live2D](http://www.live2d.com/en/)这个技术，很适合用来展示看板娘（或者叫做浮动小人、网页宠物、网页挂件之类的），折腾两天后，终于达到了一个基本可用的状态，而具体效果，在看这篇文章的你肯定已经发现了。话不多说，下面讲一下我的折腾过程：


{% cplayer %}
- 29764417
{% endcplayer %}

## 获取到可用的模型

>live2d并不是一种先进的技术，它产生的效果，都是用基本的平移、旋转、透明、曲面变形等操作实现的。最终的效果与贴图关系很大，而每一个动作，都需要制作师的精细调整。这是一个需要消耗大量时间精力的过程，因此质量好的模型并不多，质量好的也一般是在游戏中，版权受到保护，**不能随意使用**。

*关于模型的制作，日后若有机会我会另写一篇文章介绍，这里我偷个懒，直接提取手机游戏 「[药水制作师](https://play.google.com/store/apps/details?id=com.sinsiroad.potionmaker)」里Pio的模型和贴图*

### 解包是第一步

下载药水制作师的apk安装包，并使用解压缩软件解压，这款游戏是用Unity引擎开发的，因此我尝试搜索`*.assets`，发现了这个目录`\assets\bin\Data`，从包含文件判断，这也许就是我们需要的目录。
启动[Unity Studio](https://github.com/Perfare/UnityStudio)，依次点击`File-Load floder`，选择上述目录，Unity Studio会尝试读取此目录包含的所有资源文件。
读取完成后，在`Asset List`选项卡中可以看到许多资源文件。live2d的模型文件后缀名是`.moc`，动作数据后缀名是`.mtn`，在过滤器填入后缀名，可以只显示此类资源。
贴图文件较为特殊，需要按Type排序，然后一个一个查看，对于Pio来说，我们所需的贴图文件名为`default-custome`、`school-costume`和`pajamas-costume`。
![贴图文件](https://img.imjad.cn/images/2017/11/18/sp171118_135536.png)
可以看到模型文件有两个，动作数据有许多（Unity Studio会给重名资源文件名添加一个格式为`#+数字`的后缀），选中它们，依次点击`Export-Selected assets`，选择一个目录放置解包后的数据。

### 组装模型真的是很麻烦呢

新建一个`motions`目录用于存储动作数据，一个`textures`目录用于存储贴图，将上述的`.mtn`文件和贴图文件放入对应目录，目录树如下：
```
│  model.moc
│
├─motions
│      Breath Dere1.mtn
│      Breath Dere2.mtn
│      Breath Dere3.mtn
│      Breath1.mtn
│      Breath2.mtn
│      ...
│
└─textures
        default-costume.png
        pajamas-costume.png
        school-costume.png
```

新建一个名为`model.json`的文件，这个JSON用于存储模型名字，指示模型与贴图、动作的关系等，内容如下：
```json
{
	"version":"1.0.0",
	"model":"model.moc",
	"textures":[
		"textures/default-costume.png",
		"textures/pajamas-costume.png",
		"textures/school-costume.png"
	],
	"layout":{
		"center_x":0.0,
		"center_y":-0.05,
		"width":2.0
	},
	"motions":{
		"idle":[
			{"file":"motions/WakeUp.mtn"},
			{"file":"motions/Breath1.mtn"},
			{"file":"motions/Breath2.mtn"},
			{"file":"motions/Breath3.mtn"},
			{"file":"motions/Breath5.mtn"},
			{"file":"motions/Breath7.mtn"},
			{"file":"motions/Breath8.mtn"}
		],
		"":[
			{"file":"motions/Breath1.mtn"},
			{"file":"motions/Breath2.mtn"},
			{"file":"motions/Breath3.mtn"},
			{"file":"motions/Breath4.mtn"},
			{"file":"motions/Breath5.mtn"},
			{"file":"motions/Breath6.mtn"},
			{"file":"motions/Breath7.mtn"},
			{"file":"motions/Breath8.mtn"},
			{"file":"motions/Fail.mtn"},
			{"file":"motions/Sleeping.mtn"},
			{"file":"motions/Success.mtn"},
			{"file":"motions/Sukebei1.mtn"},
			{"file":"motions/Sukebei2.mtn"},
			{"file":"motions/Sukebei3.mtn"},
			{"file":"motions/Touch Dere1.mtn"},
			{"file":"motions/Touch Dere2.mtn"},
			{"file":"motions/Touch Dere3.mtn"},
			{"file":"motions/Touch Dere4.mtn"},
			{"file":"motions/Touch Dere5.mtn"},
			{"file":"motions/Touch Dere6.mtn"},
			{"file":"motions/Touch1.mtn"},
			{"file":"motions/Touch2.mtn"},
			{"file":"motions/Touch3.mtn"},
			{"file":"motions/Touch4.mtn"},
			{"file":"motions/Touch5.mtn"},
			{"file":"motions/Touch6.mtn"},
			{"file":"motions/WakeUp.mtn"}
		]
	}
}
```
其中`motions`的`idle`组是放置时的动作，按照次序依次显示，可以在这里添加更多的动作
`layout`的`center_x`字段和`center_y`字段用于偏移显示模型，日后若有显示错位可以修改此处的值

**因为重名的关系，模型、动作和贴图的关系需要在接下来的步骤中尝试出来**

### 来看看工作成果吧

>查看模型需要使用LIVE2D官方提供的`Live2D Viewer`，下载和安装方法请查看[这个网页](http://sites.cybernoids.jp/cubism2/tools/live2d-viewer)

打开Live2D Viewer，将`model.json`拖入主窗口，可以看到模型已经加载成功，若有贴图错位、动作鬼畜等情况则为上一步的`model.json`文件配置有误，测试直到找到正确的模型、动作、贴图组合
![Live2D Viewer](https://img.imjad.cn/images/2017/11/18/sp171118_143233.png)

#### 测试动作
选择要测试的动作，点击`Playback`播放动作以测试动作是否正常，勾选`Loop`可以循环播放
![测试动作](https://img.imjad.cn/images/2017/11/18/sp171118_143554.png)

### 获取更多贴图
**APK安装包内仅包含默认两个人物的三套贴图，获取更多贴图请参考以下步骤：**

1. 安装并打开药水制作师　<s>没错我是来安利游戏的</s>
2. 玩一段时间以赚到足够多的金币
3. 用金币购买喜欢的衣服（氪金也是可以的）
4. 贴图会被下载至`/mnt/sdcard/Android/data/com.sinsiroad.potionmaker/files/UnityCache/Shared`下以40位hash命名的目录，文件名为`__data`
5. 将上述文件传送至PC
6. 使用[UABE](https://7daystodie.com/forums/showthread.php?22675-Unity-Assets-Bundle-Extractor)将`__data`文件转换为`.assets`格式
7. 参照步骤 [解包](#anchor-2) 提取出贴图文件
8. 在`model.json`的`textures`字段中加入提取到的贴图

## 结语
至此模型已经基本完善，将其添加至blog将留待下一篇文章再谈
附上Pio和Tia的模型（包含两人的动作和初始的三套贴图）：[Potion-Maker.zip](https://imjad.cn/usr/themes/Moricolor/assets/live2d/potionmaker/Potion-Maker.zip)

**请注意本文中出现的“药水制作师”、“Potion Maker”字样及应用内包含的文本、模型、图片、动作数据等所有权均属于“药水制作师”作者Sinsiroad，仅供研究学习，不得用于商业用途**


## 参考

[Live2D官网](http://www.live2d.com)
[什么是Live2D](http://www.live2d.com/en/about/whats_live2d)
[Live2D Viewer - Live2D Cubism 2 中文說明書](http://www.live2d.com/usermanual/cubism2_cn/live2dviewer.html)
[药水制作师](https://play.google.com/store/apps/details?id=com.sinsiroad.potionmaker)
[Unity Studio](https://github.com/Perfare/UnityStudio)
[UABE](https://7daystodie.com/forums/showthread.php?22675-Unity-Assets-Bundle-Extractor)