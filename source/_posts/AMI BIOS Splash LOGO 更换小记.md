---
title: AMI BIOS Splash LOGO 更换小记
date: 2018-02-15 04:13:00
categories: lab
tags: [BIOS,Splash,LOGO,AMI]
urlname: ami-bios-splash-logo-replacement-notes
---
今日无事心血来潮，决定把电脑的BIOS开机动画更换一下，配电脑时曾换为长门大萌神，看了几年毕竟有些乏味。
当初更换时未留下任何记录，所用工具和图片也在后来的一次误格全盘中丢失了。这回查了点资料，参考记忆中的过程步骤成功更换，顺便把过程记录下来，为自己和他人做个参考

{% cplayer %}
- 26220167
{% endcplayer %}

## 下载相关工具
### AFU
这台电脑的主板是MSI B85M-E45，当时售价500元左右，看中性价比买的，几年下来还算稳定，没有出现过故障
这块主板搭载的是AMI公司的Aptio4 UEFI BIOS，因此到AMI官网下载固件刷新工具[AMI Firmware Update (AFU)](https://ami.com/en/products/bios-uefi-tools-and-utilities/bios-uefi-utilities/)(注意Aptio版本，较早一些的一般为Aptio4，最新的可能是Aptio V)
点击对应版本的下载地址，会弹出下载条款，完成reCAPTCHA验证后即可开始下载
下载后的压缩包包含了许可协议副本、文档、EFI启动文件、以及我们需要的afuwin，根据系统架构选择对应的版本
在这里选择afuwin64.zip，打开之后是一个文件夹，把文件夹整个解压出来

### ChangeLogo
ChangeLogo用于替换提取到的BIOS镜像的Splash LOGO。但我在AMI官网只看到了工具介绍，未找到下载地址，因此在第三方网站上找到了这一工具[ChangeLogo](http://www.bios.net.cn/down/BIOSxg/AMIBIOSxggj/466.html)，下载并解压

## 提取BIOS
打开`AFUWINGUIx64.EXE`并给予管理员权限，会显示一些基本信息
![afuwin64.png](https://img.imjad.cn/images/2018/02/15/afuwin64.png)

点击`Save`按钮，选择保存位置，即可将BIOS提取到以rom结尾的文件里，大小为6291456字节，正好6 MiB
![afuwin64_save.png](https://img.imjad.cn/images/2018/02/15/afuwin64_save.png)

*需要注意的是，在BIOS读写过程中系统会假死一段时间，鼠标和键盘将没有反应，播放的音频会鬼畜。
不必惊慌，读写操作完成后就会恢复正常*

## 制作Splash LOGO
规格：1024\*768 jpeg 大小不要太大，尽量不要超出原有的图像大小
我使用了Photoshop来制作所需的图像，过程不赘述，需要注意的是最终生成的图像分辨率一定要是1024\*768，否则会出现意料之外的裁剪、拉伸或变形。
但现在很少有4:3的屏幕了，而Splash LOGO在显示时会拉伸至满屏，会出现变形现象。
解决方法也很简单，即先做一张符合显示器比例的图片，然后将其缩放至1024\*768即可，不必担心变形，显示时会自动拉伸回去
![holo.jpg](https://img.imjad.cn/images/2018/02/15/holo.jpg "我制作的Splash LOGO")

## 替换LOGO图像
打开`ChangeLogo.exe`，点击`Load Image`按钮，选择之前步骤中提取出的BIOS镜像
![change_logo_load.png](https://img.imjad.cn/images/2018/02/15/change_logo_load.png)

*此时可以点击`Save Logo`按钮，将当前的Splash LOGO提取出来，制作Splash LOGO时应参考此图像大小和分辨率等信息*

点击`Browse`按钮，选择上一步中制作的图像文件，点击`Replace Logo`按钮替换Splash LOGO，可能会提示格式未知，不支持等信息，选是即可
![change_logo_replace.png](https://img.imjad.cn/images/2018/02/15/change_logo_replace.png)

替换完成后，点击`Save Image AS`按钮，保存替换后的BIOS镜像文件

## 刷回BIOS
>**这一步非常危险，可能导致你的计算机黑屏、死机、工作不稳定，乃至无法启动**
>**请确认自己刚才所做的每一步都正确无误后再继续操作**
>**即使操作正确，也可能会遇到意料之外的情况，刷写BIOS需要数十秒时间，在此期间不能断电**
>**本人不对您计算机的故障承担任何责任**

打开`AFUWINGUIx64.EXE`，点击`Open`按钮，选择替换后的BIOS镜像文件，不必更改窗体上的选项。点击`Flash`，将弹出确认框，确认没有其他程序会干扰BIOS更新后，选择OK，开始刷写BIOS，再次提醒：

**<center>刷写BIOS需要数十秒时间，在此期间不能断电</center>** **<center>刷写BIOS需要数十秒时间，在此期间不能断电</center>** **<center>刷写BIOS需要数十秒时间，在此期间不能断电</center>**

刷写完成后如图：
![afuwin64_flash.png](https://img.imjad.cn/images/2018/02/15/afuwin64_flash.png)

重启系统，应该就能看到更换后的Splash LOGO了

## 参考
[American Megatrends Inc.  - BIOS ⁄ UEFI Utilities](https://ami.com/en/products/bios-uefi-tools-and-utilities/bios-uefi-utilities/)
[AMI Aptio Change Logo](http://www.bios.net.cn/down/BIOSxg/AMIBIOSxggj/466.html)
[赫萝 - 萌娘百科 万物皆可萌的百科全书](https://zh.moegirl.org/zh-hans/%E8%B5%AB%E8%90%9D)