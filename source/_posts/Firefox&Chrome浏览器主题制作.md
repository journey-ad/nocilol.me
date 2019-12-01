---
title: Firefox&Chrome浏览器主题制作
date: 2018-08-18 10:52:00
categories: lab
tags: [美化,Firefox,Chrome,主题]
urlname: create-your-own-firefox-and-chrome-browser-theme
---
![♥ @ MECHURAGI](https://img.imjad.cn/images/2018/08/18/65854228_p0.png)

{% cplayer %}
- 534064291
{% endcplayer %}

## 前言
网上冲浪已经是现代人类的一种生活方式，这小小的浏览器窗口建立起了个人与大千世界的联系，但你是否偶尔会觉得浏览器窗口有些太过古板了呢？只需动动手换个主题，感觉就将大不一样。

这篇文章就将介绍Firefox和Chrome主题的制作方法


## Firefox

### 准备
Firefox主题的制作方法极其简单，准备一张3000 × 200px，格式为PNG或JPG的图片即可

值得一提的是，Firefox在显示时主要使用图片的上半部分，下半部分多半时候是看不到的，因此裁剪图片时要记得把主体部分放至上半部，另外Firefox是以右上角为原点定位图片，因此制作时要注意把不变的主体部分放至图片右侧

文件大小则要求不能超过300 KiB，如果超过的话，就在[TinyPNG](https://tinypng.com/)压缩一下吧

### 制作
打开Mozilla的[附加组件开发者中心](https://addons.mozilla.org/zh-CN/developers/)并登录，没有账号就注册一个 ~~这个我想ff的用户应该都有吧~~

点击网页中间的「[提交一个新主题](https://addons.mozilla.org/zh-CN/developers/theme/submit)」进入主题编辑页面，按提示填写主题相关信息，并在「主题设计」部分上传准备好的图片文件

勾选TOS，点击「发布主题」，审核通过之后，这个主题就将出现在附加组件商店，供人下载了

### 安装
打开主题的发布页，点击「安装主题」即可

还未审核通过的主题不能公开查看，可以在「[管理我的提交](https://addons.mozilla.org/zh-CN/developers/themes)」中找到此主题，然后点击「查看公开页面」查看并安装

## Chrome

### 准备
相对来说，Chrome的主题制作起来就很麻烦了，Chrome的主题只能以扩展的形式安装

我们使用ThemeBeta这个网站进行制作

至少准备一张图片，不同区域的尺寸参考数据如下：
>- theme_frame: ∞ x 30px
>- theme_toolbar: ∞ x 120px
>- theme_tab_background: ∞ x 65px
>- theme_ntp_background: Recommended Minimum Size for images 800 x 600px
>- theme_frame_overlay: 1100 x 40px
>- theme_button_background: 30 x 30px

~~实际上并没有什么卵用，Chrome的尺寸和定位很迷，部分情况下甚至还会镜像处理，建议自己尝试修改~~

~~最蠢的是以左上角为原点进行定位，高分屏下简直就是噩梦~~

### 制作

打开[Theme Creator](https://www.themebeta.com/chrome-theme-creator-online.html)，按提示上传图片或修改颜色即可，右侧有实时预览，不过分辨率并不高，只能作为布局参考

每完成一部分后就点击「Pack and Install」打包并安装以进行测试，注意下载时会有安全提示，继续即可

最终完成后点击「Save Online」，即可发布至主题商店供人下载

## 我制作的
基于[無題/PSD](https://www.pixiv.net/member_illust.php?mode=medium&illust_id=50973239)制作

### Firefox
[Nico & Maki](https://addons.mozilla.org/zh-CN/firefox/addon/nico-maki/)

![Firefox](https://img.imjad.cn/images/2018/08/18/sp180818_201120.png)

### Chrome
[Nico & Maki](https://www.themebeta.com/chrome/theme/1035989)

![Chrome](https://img.imjad.cn/images/2018/08/18/sp180818_201408.png)