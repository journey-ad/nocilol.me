---
title: 关于如何在网页中引入思源字体：漫谈Typekit
date: 2018-07-02 06:13:00
categories: lab
tags: [字体,设计,Typekit,思源字体]
urlname: how-to-introduce-source-han-fonts-into-web-pages-through-typekit
---
![Source Han Serif](https://img.imjad.cn/images/2018/06/29/sp180629_130227.png)

## 前言
在[上一篇文章](https://imjad.cn/archives/lab/apply-different-styles-when-printing-a-web-page)里，我曾提到了将正文字体更换为开源免费的思源宋体这一做法，本文就将介绍通过Typekit服务在网页中引入思源字体的方法，并讲述一些思源字体的历史

可以点击这里<a href="javascript:switchSerifMode()" target="_self">查看效果</a>


*为避免争议，除特别说明外，此文中的思源字体仅指**思源黑体**与**思源宋体***

## Typekit

### 注册
*已有账号请忽略此步骤*
1. 打开[Typekit官网](https://typekit.com/)，点击右上角的「Sign in」打开登录页
2. 点击「Get an Adobe ID」进入注册页
3. 按要求输入姓名、邮箱、密码、国别和生日等信息
**由于Typekit服务并未进入中国大陆市场，此处国别区域一定不要选China，否则会在登录时出现Access Denied的问题，我选择了Hongkong**
4. 点击「Sign up」按钮，Adobe会发送一封邮件用来验证邮箱，点击邮箱链接完成验证

### 登录
在登录页输入帐号密码登录即可，若出现**Access Denied**的提示，请重新注册账号，并勾选国别区域为非中国大陆

### 挑选字体
1. 搜索*source-han-serif-simplified-chinese*找到[思源宋体简体版本](https://typekit.com/fonts/source-han-serif-simplified-chinese)的Typekit页面，注意不要选错为*source-han-serif*，后者为仅包含西文字体的版本
2. 点击「ADD TO KIT」按钮
3. 点击「CREATE A KIT」按钮以创建一个新的KIT（free plan仅能创建一个kit）
4. 页面会提示输入KIT名称和域名白名单，可用通配符
5. 随后页面会打开一个小窗口，所有的配置都将在小窗口内完成

### 配置KIT
![KIT 配置](https://img.imjad.cn/images/2018/06/29/sp180629_114731.png)

如图所示，左侧为所选字体的配置项，中间为此Kit的字体列表，右下角为发布按钮，任何改动若想生效必须点击此按钮进行发布

点击左上角的「Using fonts in CSS」可以看到详细的CSS配置方法

默认设置几乎不需进行任何改动，可能需要改动的是「Weights & Styles」区域，即字重配置。除非特殊情况，建议保持默认防止字体加载速度过慢

点击右上角的「Kit Settings」可以更改Kit名、域名白名单列表，或是删除此kit。若字体不经常发生变动建议勾选「Optimize performance」启用长缓存优化访问速度

点击右上角的「Embed Code」可以查看在页面内引入Typekit服务的代码，分为**Default**和**Advanced**两种

**Default**直接在页面的HTML结构中插入即可，但这种方法为同步加载，在JS脚本下载并执行完成前会阻止页面渲染，因此应尽量放至页面的footer区域或是在script标签中加入async参数将其指定为异步加载

**Advanced**为动态插入script标签并异步加载，异步加载可以不影响页面渲染，但会有一段时间的未替换字体状态并导致页面字体替换时发生闪烁

鱼与熊掌不可得兼，加载慢但直接显示为替换后的字体或页面加载完成后替换，根据自己的需求选择即可

### 插入代码
将需要替换字体的元素的样式`font-family`设置为`"source-han-serif-sc"`，这里是一个完整的示例：
```css
p, li {
    font-family: 'Source Han Serif SC','Source Han Serif','source-han-serif-sc','PT Serif','SongTi SC','MicroSoft Yahei',Source Han Sans SC,Noto Sans CJK SC,WenQuanYi Micro Hei,sans-serif;
}
```

在页脚区域插入上一步中获取到的代码，即可在网页内加载思源字体

## 一点历史

### 关于思源字体
思源字体是Adobe与Google所领导开发的开放源代码字体家族中的泛 CJK 字体，目前已有**思源黑体**与**思源宋体**两种字体，涵盖了简体中文、繁体中文、日文和韩文在内的所有文字，并且各有7种字重（ExtraLight 250、Light 300、Normal 350、Regular 400、Medium 500、Bold 700、Heavy 900）

西文部分为来自Source Sans（或Source Serif）字体家族的拉丁文、希腊文和西里尔文字形。总收录字形达65,535个之巨（OpenType格式支持的最大上限），整个字体家族的字形个数接近50万

思源字体名称中的「思源」一词来自于成语「饮水思源」，目前以SIL开源字体授权。Adobe提供了多语言OTF版、各地区子集版、多语言OTC版等多个版本的下载和TypeKit服务，下载版不是本文重点，因此不作介绍，具体请看Adobe的[下载指南](https://www.adobe.com/content/dam/acom/en/products/type/pdfs/download-guide-source-han.pdf#page=4)

>Google将此两个字体另起名为**Noto Sans CJK**与**Noto Serif CJK**发布，隶属Noto字体家族，但字形本身（包括西文、数字）与思源字体完全一样，仅字重称呼不同。
>「Noto」一词意为[不再有豆腐]（no more tofu），这是由于计算机无法显示的字体，会出现空白的方格，这些方格又称为「豆腐」。
>Noto字体的目的就是要消灭这些豆腐（意即尽可能地补上缺少的字符）

### 关于Typekit
Typekit是一家知名的在线字体提供商，于2011年被Adobe公司收购，并被Adobe与其家的Creative Cloud整合起来，用户可以以简单的方式使用Adobe的正版字体及TypeKit的在线字体功能

## 结语
思源系列字体以其开源免费的协议和丰富的字重使无数的设计师趋之若鹜，而收录齐全这点绝对是一项大工程，这点不得不感谢Adobe和Google的贡献，开源社区的力量在商业公司面前还是太过薄弱了

英文字体的Web Font服务应用已久，但中文的Web Font得不到推广，一个很大的原因就是中文字符太多，动辄4、5M的字体文件大小确实让人吃不消

好在有Typekit这类专业的Web Font服务提供商，通过各种优化加载，使中文Web Font的应用得到了推广的可能，在避免影响性能的前提下，为我们的页面增色不少

此外也有一些其他的方案，如「[字蛛](http://font-spider.org/)」等，通过分析本地CSS与HTML文件获取Web Font中没有使用的字符，并将这些字符数据从字体中删除以实现压缩，同时生成跨浏览器使用的格式。在此不作详细探讨

## 关于
[Typekit](https://typekit.com/)
[思源字体下载指南](https://www.adobe.com/content/dam/acom/en/products/type/pdfs/download-guide-source-han.pdf#page=4)
[Web 中文字体应用指南](https://ruby-china.org/topics/14005)