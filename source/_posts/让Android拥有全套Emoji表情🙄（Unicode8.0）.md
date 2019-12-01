---
title: 让Android拥有全套Emoji表情🙄（Unicode8.0）
date: 2016-07-10 13:36:00
categories: lab
tags: [android,安卓,emoji,字体,表情,Unicode]
urlname: let-android-have-a-full-set-of-emoji-expression--unicode80
---
***!!!任何对系统文件进行修改的操作都可能造成严重的后果，请确认你已拥有足够的知识和谨慎的头脑再进行以下操作!!!***

```
我的环境:
型号 Redmi Note3
Android版本 Android 5.0.2
```
## 1.备份
备份```/system/fonts/NotoColorEmoji.ttf```文件
可使用RE管理器或MT管理器等应用

## 2.下载字体文件
分别为Android 6提取文件和iOS 9.1提取文件， 看自己喜好，个人感觉安卓的更萌一些
务必检查MD5！
```
305da95e709e98e6664fa7059f1ca35d  android 6/NotoColorEmoji.ttf
9c4c5d148a4a39622f507a68c24903e9  ios9.1/NotoColorEmoji.ttf
```
[百度云](http://pan.baidu.com/s/1c2x9aPq "百度云")

## 3.安装
- 将```/system/fonts/NotoColorEmoji.ttf```替换为下载好的Emoji字体文件
- 修改权限为644，如下图
- 重启系统
- 清除输入法数据，enjoy~

![chmod](https://img.imjad.cn/images/2016/07/10/Screenshot_2016-07-10-23-15-27.md.jpg "chmod644")

## 4.其他
测试显示是否正常可以在[这里](http://emojipedia.org/unicode-8.0/ "这里")
拼音输入法个人推荐谷歌拼音输入法和手心输入法。
谷歌的特点是简洁、单手模式和特色的滑行输入。权限要求不像国内的输入法那么多，词库初期有些不足，但经过调教后打字十分爽。前段时间更新的新版支持了自定义皮肤，如果对功能要求不多无疑十分适用。
（下图为谷歌拼音输入法+ IOS Emoji）
![谷歌拼音输入法](https://img.imjad.cn/images/2016/07/10/Screenshot_2016-07-09-16-16-36.jpg "谷歌拼音输入法")

手心特点是功能全而不繁，界面称得上漂亮二字，词库和联想功能强大，支持词库同步等。另外手心和360有着千丝万缕的联系，介意勿用。
（下图为手心输入法+ Android Emoji）
![手心输入法](https://img.imjad.cn/images/2016/07/10/Screenshot_2016-07-10-22-09-38.jpg "手心输入法")


参考资料：
[让 Android 拥有全套 Emoji 表情的解决方案 | 有用功](http://www.ifanr.com/app/609625 "让 Android 拥有全套 Emoji 表情的解决方案 | 有用功")
[探索在Android中使用Emoji Font的方法](http://ragnraok.github.io/android-emoji-font-method.html "探索在Android中使用Emoji Font的方法")
[Unicode Version 8.0](http://emojipedia.org/unicode-8.0/ "Unicode Version 8.0")
感谢[@Ali](https://ilii.me/ "@Ali")提取的Android 6 Emoji字体文件