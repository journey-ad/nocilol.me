---
title: MIDO Splash LOGO 更换小记 
date: 2018-02-16 02:02:00
categories: lab
tags: [android,Splash,LOGO]
urlname: mido-splash-logo-replacement-notes
---
昨天介绍了[更换AMI BIOS Splash LOGO的方法步骤](https://imjad.cn/archives/lab/ami-bios-splash-logo-replacement-notes)，今天接着介绍更换手机Splash LOGO的方法。对于不同型号的手机所需的工具可能不太一样，在这里以我的红米Note4X高通版(mido)为例。其它型号请参考[这个帖子](https://forum.xda-developers.com/android/software-hacking/guide-how-to-create-custom-boot-logo-t3470473)


## 什么是Splash LOGO
Splash LOGO不是开机动画，而是手机开机后一段时间内显示的图像，最常见的是手机品牌的商标。对于MIDO（或者说小米设备）来说，这个图标就是常说的“MI”图标

## 备份（可选）
虽说备份总是必不可少的，但这次却并不是很有必要，因为可以从官方固件中提取到，但为了以防万一，还是备份一下吧~
在手机的终端模拟器中执行`dd if=/dev/block/bootdevice/by-name/splash of=/sdcard/splash.img`，即可将Splash LOGO备份至SD卡

## 下载制作工具
感谢XDA大神[@GokulNC](https://forum.xda-developers.com/member.php?u=6319272)的[教程和Splash LOGO制作工具](https://forum.xda-developers.com/redmi-note-4/how-to/guide-how-to-change-boot-logo-splash-t3572441)。
前人栽树后人乘凉，我们不必关心具体的技术细节，只需掌握基本的文件操作就能够实现制作和更换Splash LOGO
下载并解压**Redmi_Note_4_Splash_Maker.zip**

## 准备图片
图片分辨率应小于1080\*1920
另外MIDO分配给Splash LOGO的空间仅有可怜的100KiB，因此只能使用非常小的图片
*如果在脚本中更改限制，fastboot图片将不会在fastboot模式中显示*
作者的建议是仅使用有限的两种或三种颜色的图片，比如黑白图片。图片会居中以黑底显示，因此过大时可通过裁剪来减小图片体积
另外推荐[TinyPNG](https://tinypng.com)，通过自创的压缩算法，可极大压缩图片体积
这是我制作的LOGO，可以看到经过裁剪及压缩后，大小仅有790字节![logo.png](https://img.imjad.cn/images/2018/02/15/logo.png)

根据格式将图片重命名为logo.png或logo.jpg，将其放到`pics`目录中。

## 制作镜像文件
执行`CREATE_LOGO.bat`，稍等数秒，output目录中即可生成`splash.img`。
脚本会提示是否创建卡刷包，输入yes并回车，output目录中即可生成`flashable_splash.zip`。
卡刷包可用自定义Recovery刷入
![bat_flashable.png](https://img.imjad.cn/images/2018/02/15/bat_flashable.png)

## 刷写到设备
有三种方法，选择一种你认为最简单的：

### 从rec卡刷
进入rec卡刷上一步生成的`flashable_splash.zip`即可
### 通过终端模拟器写入
将`splash.img`传送至手机，打开终端模拟器，执行以下命令：
```shell
su
dd if=/sdcard/splash.img of=/dev/block/bootdevice/by-name/splash
```
### 通过fastboot刷入
连接至电脑，打开adb工具，使用fastboot命令刷入：
```shell
fastboot flash splash splash.img
```

重启手机，应该就能看到更换后的Splash LOGO了

## 参考
[[GUIDE] How to Change Boot Logo (Splash Screen) for Snapdragon Devices (splash.img)](https://forum.xda-developers.com/android/software-hacking/guide-how-to-create-custom-boot-logo-t3470473)
[[GUIDE] How to Change Boot Logo (Splash Screen) for Redmi Note 4](https://forum.xda-developers.com/redmi-note-4/how-to/guide-how-to-change-boot-logo-splash-t3572441)