---
title: 使用Cool Retro Term把你的终端变成复古显示器
date: 2017-12-02 09:37:00
categories: lab
tags: [linux,终端,复古,极客]
urlname: use-cool-retro-term-to-turn-your-terminal-into-a-retro-display
---
![Screenshot-20171202152208-1284x746.png](https://img.imjad.cn/images/2017/12/02/Screenshot-20171202152208-1284x746.png)

## 前言
CRT(阴极射线管)显示器曾见证了计算机技术发展的黄金时代。但由于笨重、耗电高、较占空间等种种缺点，在轻薄、功耗低的LCD(液晶显示器)发明并得到推广后被迅速取代，如今我们只能在电影和游戏中找到它们的身影。

Cool Retro Term使我们能够暂时「回到过去」，看看上世纪八十年代流行的显示器是什么样子。



>[Cool Retro Term](https://github.com/Swordfish90/cool-retro-term)是一个设计上模仿古老的阴极射线管显示器的终端模拟器。它被设计的十分养眼、可定制和相当轻巧。

>它由[Swordfish90](https://github.com/Swordfish90)使用QML开发

>这个终端模拟器在Linux和macOS下工作，并且需要Qt 5.2及以上版本支持

{% cplayer %}
- 520459951
{% endcplayer %}

## 安装
这里只介绍Linux上的安装方法，macOS请参考[文档](https://github.com/Swordfish90/cool-retro-term/blob/master/README.md#build-instructions-osx)说明或是下载编译好的[DMG](https://github.com/Swordfish90/cool-retro-term/releases)

### 使用包管理器
如果你是ArchLinux或Ubuntu用户，可以使用包管理器进行安装，避免繁杂的编译安装过程。

#### ArchLinux

ArchLinux的官方仓库中已经包含了Cool Retro Term，执行以下命令安装：
```bash
# pacman -S cool-retro-term
```
或者从AUR安装开发版本：
```bash
$ yaourt -S cool-retro-term-git
```
#### Ubuntu
从PPA安装
```bash
# add-apt-repository ppa:noobslab/apps
# apt-get update
# apt-get install cool-retro-term
```
### 编译安装
#### 依赖
进行编译之前，先要安装一些依赖。不同的发行版所需的依赖不太一样，具体可以参考文档的[Dependencies](https://github.com/Swordfish90/cool-retro-term#dependencies)部分

对于ArchLinux来说，应安装这些依赖：
```bash
# pacman -S qt5-base qt5-declarative qt5-quickcontrols qt5-graphicaleffects
```

#### 编译
依赖装好之后，就可以开始编译了。执行以下命令：
```bash
# 从Github克隆到本地
$ git clone --recursive https://github.com/Swordfish90/cool-retro-term.git

# 开始编译(Fedora 和 OpenSUSE 用户需要使用 qmake-qt5 替换 qmake)
$ cd cool-retro-term
$ qmake && make
```

程序编译成功后，就可以使用下面的命令来运行它了：
```bash
$ ./cool-retro-term
```

#### 添加至程序菜单
```bash
# cp cool-retro-term.desktop /usr/share/applications
```

## 使用
程序带有一些命令行参数，执行`cool-retro-term -h`可以查看这些参数：
```
"Usage: cool-retro-term [--default-settings] [--workdir <dir>] [--program <prog>] [-p|--profile <prof>] [--fullscreen] [-h|--help]"
  --default-settings  Run cool-retro-term with the default settings
  --workdir <dir>     Change working directory to 'dir'
  -e <cmd>            Command to execute. This option will catch all following arguments, so use it as the last option.
  --fullscreen        Run cool-retro-term in fullscreen.
  -p|--profile <prof> Run cool-retro-term with the given profile.
  -h|--help           Print this help.
  --verbose           Print additional information such as profiles and settings.
```

打开Cool Retro Term后，默认呈现的就是一台古老的CRT显示器的样子。若不喜欢默认样式，点击Edit-Settings可以进行设置。

Genreal选项卡里的Profile有一些自带的配置文件，点击Load按钮可以加载它们。点击New新建一个配置文件，然后可以进行具体的配置，调整效果实时可见，十分方便。

![Screenshot-20171202171146-1346x733.png](https://img.imjad.cn/images/2017/12/02/Screenshot-20171202171146-1346x733.png)

### 我的配置文件
```javascript
{
  "ambientLight": 0.05,
  "backgroundColor": "#000000",
  "bloom": 0.55,
  "brightness": 0.5,
  "burnIn": 0.4,
  "chromaColor": 0,
  "contrast": 0.85,
  "customCommand": "",
  "flickering": 0.05,
  "fontColor": "#ff8100",
  "fontName": "APPLE_II",
  "fontWidth": 0.95,
  "frameName": "SIMPLE_WHITE_FRAME",
  "glowingLine": 0.25,
  "horizontalSync": 0.15,
  "jitter": 0.1,
  "rasterization": 2,
  "rbgShift": 0,
  "saturationColor": 0.1,
  "screenCurvature": 0.1,
  "staticNoise": 0.1,
  "useCustomCommand": false,
  "windowOpacity": 1,
  "name": "Wasteland"
}
```

## 几张截图
![Screenshot-20171202012048-1920x1080.png](https://img.imjad.cn/images/2017/12/02/Screenshot-20171202012048-1920x1080.png)

![Screenshot-20171202172553-1423x770.png](https://img.imjad.cn/images/2017/12/02/Screenshot-20171202172553-1423x770.png)

![Screenshot-20171202172435-1423x770.png](https://img.imjad.cn/images/2017/12/02/Screenshot-20171202172435-1423x770.png)

![Screenshot-20171202153626-1419x804.png](https://img.imjad.cn/images/2017/12/02/Screenshot-20171202153626-1419x804.png)

![Screenshot-20171202012701-1920x1080.png](https://img.imjad.cn/images/2017/12/02/Screenshot-20171202012701-1920x1080.png)