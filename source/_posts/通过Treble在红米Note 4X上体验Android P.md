---
title: 通过Treble在红米Note 4X上体验Android P
date: 2018-07-16 14:04:00
categories: lab
tags: [android,安卓,刷机,Treble]
urlname: experience-android-p-dp3-on-mido
---
## 更新
>2018-8-8
>Android Pie正式版镜像已释出
>~~~2018-7-20~~~
>~~~可用的DP4镜像已释出~~~
>~~~Magisk最新测试版本已支持Treble~~~
>~~~相机可用改版Google Camera~~~

## 关于Treble
Android系统的碎片化一直是令Google和开发者颇为头疼的一件事。相对于iOS的封闭，Android在开放的同时也意味着更多的风险和不可控性，从软件的碎片化——基于不同版本的系统开发的各式应用或定制系统，其中有一些用的还是Android 4.x甚至2.x这种古老的版本，到硬件的碎片化——不同的处理器架构、不同的性能、不同的交互方式，甚至连屏幕的材质和尺寸都可以千奇百怪。在因开放市占率傲视群雄的同时，也因此深受碎片化带来的混乱之苦。

Google显然也意识到了这一点，开始逐渐收紧对Android的把控，如收紧权限管理、禁止调用私有API等。但正因为碎片化，设备适配新版本系统困难，这一策略并不理想。


![Android版本碎片化严重](https://img.imjad.cn/images/2018/07/16/sp180716_230403.png)


但如今事情起了变化，Treble Project的推出，降低了手机厂商适配新版本Android系统的难度。手机厂商可以专注于系统的功能和体验，而不是像以前一样，必须等待硬件厂商的驱动更新后将其整合进系统，同时伴随着大量的底层开发和测试。（这种升级方式又进一步加剧了Android的碎片化）

对第三方OS开发者而言，Treble无疑也是好事一桩，这意味着能真正意义上做出一款通刷的刷机包了

部分手机厂商的态度目前仍显得比较暧昧，支持Treble意味着用户可以简单的更换手机系统，不甘心「只卖手机」的厂商对Treble不感兴趣也是可以理解的了

幸运的是，有民间大神为一些设备制作了第三方的Treble支持，这些支持被戏称为「土制Treble」。虽然只是「土制」，但使用体验上却没什么区别

我手中的这台红米Note 4X高通版「代号MIDO」，就使用了由Lineage OS的开发团队制作的Treble支持。并由此成功吃上了Android P

## 安装过程
在此不探讨解BL锁、刷REC、救砖等问题。这些都是搞机所必需了解的知识，网络上有很多相关文章和教程，可以自行搜索

*进行刷机操作时，良好的操作习惯和清醒的头脑是必要的，本人不对您手机的故障承担任何责任*

此刷机包基于Android P DP3制作，由Telegram群组[@P semi-GSI](https://t.me/PsemiGSI)提供，本文中提到的系统镜像等资源均可在此群组的固定消息或群组文件中找到，发现Bug亦可向此群组进行报告

### 在REC的操作
*此处REC均使用**TWRP**进行操作*

0. (可选)开启vendor支持
    擦除`system`、`data`、`cache`、`dalvik-cache`、`vendor`分区，并刷写对应系统以开启vendor支持，若已支持可跳过此步
    我刷的是[AEX5.7](https://forum.xda-developers.com/redmi-note-4/xiaomi-redmi-note-4-snapdragon-roms-kernels-recoveries--other-development/rom-aospextended-rom-v5-0-t3679746)，这也是我此前日用的系统，其他可选有LOS/MoKee等，按各系统的发布说明进行刷机操作
1. 擦除数据
    大版本升级，需要擦除`system`、`data`、`cache`、`dalvik-cache`分区，请提前做好数据备份
    *注意上一步产生的`vendor`分区在这一步里应予以保留，不要手滑擦掉了*
2. 刷写系统
    下载**P-DP3-sGSI.img.zip**，解压其中的`P-DP3-sGSI.img`并刷写至`system`分区
3. 刷写semi-GSI
    挂载`vendor`分区，并按1~3的次序依次刷入**POST-sGSI**的ZIP包
4. 重启至系统

### 一些Bug的修正与规避
上一步重启至系统后，一切正常的话就能看到Android P的真容了，但由于这个系统并非为单个型号手机配置，出现一些Bug也是在所难免。比如我就遇到了动画卡顿、程序假死、偶发FC的问题，下面就将对一些常见的问题进行解决：

*在上一步中不要偷懒，完整四清，一个干净的环境能减少一些疑难杂症的发生概率*

#### 启动器FC
通过**ADB**安装第三方启动器，如Nova
    
#### Play商店无法使用
在Apkmirror下载对应处理器架构的GMS的安装包并安装，对于MIDO，一个典型的名字是`com.google.android.gms_12.8.72_(100400-202717283)-12872041_minAPI28(arm64-v8a,armeabi-v7a)(nodpi)_apkmirror.com.apk`

#### 迷之卡顿
下载**p-lag-fix.zip**并选择刷入

#### 指纹传感器异常
下载**Fix_FP_Scanner.zip**并选择刷入

#### mtp不能使用
开启「开发者选项」并进入，往下拉找到默认USB配置，选择「文件传输」

#### 导航栏的最近任务按钮变成了菜单按钮
编辑`/system/usr/keylayout/Generic.kl`，搜索`MENU`，可能会有多个结果，将`MENU`修改为`APP_SWITCH`

#### 无法设置mp3格式铃声
使用第三方播放器的铃声设置功能

### 一些拓展应用

#### Magisk
下载**Magisk-v16.4-Treble.zip**并选择刷入

### 一些额外操作

#### 安装相机
此系统没有自带相机，请安装[Opencamera](https://play.google.com/store/apps/details?id=net.sourceforge.opencamera)来代替，注意不支持C2API

#### 禁用导航栏
在`/vendor/build.prop`中加入一行`qemu.hw.mainkeys=1`并重启

#### 更换Captive Portal Server地址
连接ADB，执行：
```shell
adb shell "settings put global captive_portal_http_url http://captive.v2ex.co/generate_204"; 
adb shell "settings put global captive_portal_https_url https://captive.v2ex.co/generate_204";
adb shell "settings put global captive_portal_fallback_url http://captive.v2ex.co/generate_204";
adb shell "settings put global captive_portal_other_fallback_urls http://captive.v2ex.co/generate_204"; 
```

## 体验
*待补充*

## 一些截图
![媒体音量调整](https://img.imjad.cn/images/2018/07/17/Screenshot_20180717-082136.png)

![通知栏](https://img.imjad.cn/images/2018/07/16/Screenshot_20180716-211546.png)

![截图编辑功能](https://img.imjad.cn/images/2018/07/17/Screenshot_20180717-082310.png)

![磁贴](https://img.imjad.cn/images/2018/07/16/Screenshot_20180716-211552.png)

![最近任务列表](https://img.imjad.cn/images/2018/07/17/Screenshot_20180717-082104.png)

![设置界面](https://img.imjad.cn/images/2018/07/16/Screenshot_20180716-211627.png)

![系统界面](https://img.imjad.cn/images/2018/07/16/Screenshot_20180716-211713.png)

![系统版本](https://img.imjad.cn/images/2018/07/16/Screenshot_20180716-211736.png)

![P的彩蛋](https://img.imjad.cn/images/2018/07/16/Screenshot_20180716-211755.png)