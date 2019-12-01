---
title: 给博客添加能动的看板娘(Live2D)-模型格式v3转v2
date: 2018-09-12 18:18:00
categories: lab
tags: [Live2D,看板娘]
urlname: add-dynamic-poster-girl-with-live2d-to-your-blog-03
---
![ころんびょん血小板ちゃん @ 白木栞](https://img.imjad.cn/images/2018/09/13/70257184_p0.jpg)

{% cplayer %}
- 1297743786
{% endcplayer %}

## 发现血小板

最近在网上冲浪时，发现了一位大佬发布了『工作细胞』中血小板的模型 [原推地址](https://twitter.com/DenchiSoft/status/1036017773011525632)。作为萝莉控的绅士，我不由得春心萌动起来，更令人惊喜的是作者数天后在评论区放出了模型的源文件，实在是太令人感动了。于是事情很快就明朗了：陪伴了玩水半年之久的Pio，被无情的打入了冷宫

*这位大佬看资料是德国人*

[模型源文件](https://www.dropbox.com/s/4lbkvip48k3q83o/kesshouban.zip)


## 二选其一

由于之前的轮子仅支持Cubism 2版（下文简称为v2）格式的模型，而这次发现的模型为Cubism 3版（下文简称为v3）。所以摆在我们面前的有两个选择：一是升级SDK到v3，二是将v3格式的模型转为v2格式

而升级SDK不是个轻松的工作，既然已经有大佬[开了坑](https://github.com/xiazeyu/live2d-widget.js)，我这半吊子水平就不班门弄斧了。但问题又来了，live2d-widget.js的作者[@xiazeyu](https://github.com/xiazeyu)目前要准备高考，短时间内无法更新（画外音：果然是大佬）。

而血小板模型的作者在发布时说了「ご自由に使ってどうぞ～！」，所以我们可以丢掉擅自修改模型的包袱。综合考虑一下，那么就选择第二条路吧


## 准备工作

工欲善其事，必先利其器，首先我们需要下载需要用到的软件和工具


### Cubism 3 Editor

访问[Live2D官网](http://www.live2d.com)然后找到Cubism 3 Editor的发布页并进入[下载页](http://cubism3.live2d.com.s3-website-ap-northeast-1.amazonaws.com/download_en.html)，勾选同意使用协议并输入邮箱后就可以下载了，最新版的就可以，然后正常安装


### 可选

必要的软件其实就Cubism 3 Editor一个，但为了方便考虑，可以选择安装下列软件工具


#### Cubism 3 Viewer

[下载页](http://cubism3.live2d.com.s3.amazonaws.com/tools/cubism3viewer/download.html)


## 开始行动

### (可选)查看模型

打开Cubism 3 Viewer，映入眼帘的是经典的Unity游戏启动界面，简单设置分辨率等后进入主界面。

按钮功能都很简单明了，点击**Load Json**加载模型JSON

![Cubism 3 Viewer](https://img.imjad.cn/images/2018/09/13/sp180912_234533.png)


打开Cubism 3 Editor，因为这个模型材质图的分辨率高达4096 px，且部件数量远超出免费版的限制，因此我们只能使用专业版来操作。好在专业版有42天的试用时间，试用期间功能与正常版本无异

将`kesshouban/model/kesshouban_v1.1.cmo3`拖入编辑器，即可进入编辑状态：

*这里不得不吐槽一句，都8102年了竟然还不支持高分屏，显示效果真的辣眼睛*

![Cubism 3 Editor](https://img.imjad.cn/images/2018/09/13/sp180912_234817.png)

可以看到密密麻麻的网格与活动部件，这些都是一个优秀的模型的基础，背后是大量时间与精力的付出，大家可以以此模型为参考学习制作Live2D模型

出于多种原因，绝大部分制作师发布时只会发布导出后的模型文件。肯将作品的源文件分享出来的大佬，真的是太令人感动了

闲话少叙，让我们继续下一步


### 修改材质图

这块的操作与个人喜好有关。比如我觉得血小板背后的路障标志和脚下的地面不适合在网页上显示，帽子的字体也不是很喜欢，那么就来修改材质图吧

首先进行分析分析，上面说的要修改的三点实际上可分为两种情况：

1. 对显示结果中的部分区域做修改
2. 隐藏或删去最终显示结果的一部分

对于第一种情况，只能对材质图本身作修改了，先从导出材质图开始吧


#### 导出材质图

依次点击**Modeling** - **Texture** - **Edit Texture Atlas**打开材质图编辑窗口

然后点击**File** - **Export Texture Image**，或点击<kbd>Ctrl+S</kbd>，然后选择材质图保存目录即可导出，导出后即可用一般的图像处理软件进行修改，注意不要对材质图进行任何的移动或裁剪

#### 替换材质图

在材质图编辑窗口内点击**File** - **Replace Texture Image**，或点击<kbd>Ctrl+O</kbd>，然后选择材质图即可导入，导入后无需更多操作，直接点击**OK**关闭窗口，软件会刷新数据，稍等片刻即可


第二种情况可以用两种方法，一是将目标区域的材质图修改为透明，那么在显示时自然就会「消失」，二是删除目标区域部分的模型。为了减少模型文件大小和减轻运算量，推荐使用第二种方法。当然第一种方法并不是不可以，在无法接触到模型的源文件时，这也是唯一的方法。

在主界面内点击想要删除的部件，出现红框即代表此部件被选中，然后点击<kbd>Del</kbd>键删除

![删除部件](https://img.imjad.cn/images/2018/09/13/sp180913_002734.png)


### 转换部位ID

由于v3格式与v2的部位ID不同，直接导出的话会出现不跟随鼠标动作等问题，需要把v3格式的部位ID转为v2格式，编辑器已经包含了这个功能：

在主窗口中依次点击**Modeling** - **Convert Model ID**打开ID转换窗口

![ID转换窗口](https://img.imjad.cn/images/2018/09/21/sp180921_131428.png)

保持默认，点击**OK**就可以了


### 导出模型

材质图和模型修改完成后，就可以开始模型的导出工作了

在主窗口中依次点击**File** - **Export For Runtime** - **Export as moc file (for 2.1)**打开导出窗口

![导出窗口](https://img.imjad.cn/images/2018/09/13/sp180913_005103.png)

值得注意的是最下面的材质图分辨率设定。由于本次的最终目标是在网页上展示，所以综合加载速度与分辨率，2048 px是一个较为推荐的选择

点击**OK**，选择目录完成导出


此时v2格式的模型已经初步可用，可以放到页面上试试啦

*不知为何v2的Live2D Viewer无法正常显示导出的模型，会显示为白屏，但在页面上能正常显示出来，期望得到高人解答*


### 转换动作数据

但此时会发现虽然血小板能正常显示了，却只是呆呆的站着，点击也没有任何反应，这是缺失[动作]^(Motion)数据的缘故。

观察后我们发现作者已经给出了四个动作数据，分别为[阿诺内阿诺内]^(Anone_Synced)、[跳舞]^(Dance)、[闲置]^(Idle)和[睡觉]^(Nemui)，但是却是v3格式的，那么就来把它们转为v2格式的吧


回到主界面，将`kesshouban/model/kesshouban_v1.1.can3`拖入编辑器，这个文件便是v3的动作数据源文件

打开时可能会提示找不到cmo3文件，点击**Replace**然后找到对应的模型文件即可

![动作数据编辑](https://img.imjad.cn/images/2018/09/13/sp180913_013856.png)

在左侧中部的Scene列表中即可看到录制好的四个动作数据，可以通过中下部时间轴的播放控制条来播放动作数据

依次点击**File** - **Export For Runtime** - **Export motion file**打开导出窗口，勾选**Output All Scenes**将导出所有动作数据，勾选**Output Selected Scenes**则导出Scene列表里选中的动作数据

注意勾选最下方的**Export as 2.1-format file**，然后点击**OK**完成导出

![动作数据导出](https://img.imjad.cn/images/2018/09/13/sp180913_014250.png)


## 大功告成

那么差不多就是这样了，~~虽说转换后的模型仍有一些问题，比如不跟随鼠标指针，但总体来说仍算成功。~~此外编辑器还提供了一些其他的有意思的功能，可以自己试试看

关于不跟随鼠标指针，是v3格式与v2的部位ID不同导致的，转换方法已在文中补充，具体请参考[转换ID](http://docs.live2d.com/cubism-editor-manual/convert-id/)

接着就是配置到网页上了，配置方法和普通v2格式的模型配置方法差不多，有需要的话请参考[此前的文章](https://imjad.cn/archives/lab/add-dynamic-poster-girl-with-live2d-to-your-blog-01)

按照惯例放出未经修改的的v2格式模型：[下载地址](https://cdn.imjad.cn/usr/uploads/kesshouban_v2.7z)

那么在最后祝@xiazeyu大佬能顺利完成学业，继续为我们带来好用的工具

感谢@DenchiSoft带来的作品，血小板，她真的棒极了！