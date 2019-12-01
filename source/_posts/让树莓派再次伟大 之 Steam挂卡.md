---
title: 让树莓派再次伟大 之 Steam挂卡
date: 2018-04-06 17:11:00
categories: lab
tags: [树莓派,Steam,游戏,挂卡,ASF]
urlname: make-raspberry-pi-great-again--steam-farm
---
![Screenshot-20180407025923-1920x1080.png](https://img.imjad.cn/images/2018/04/07/Screenshot-20180407025923-1920x1080.png)
## 前言
近日从抽屉里找回了吃灰已久的树莓派，本着物尽其用的原则，将其作为steam挂卡机器是个不错的选择

steam挂卡工具有许多，如IdleMaster、ASF、SAM等。其中最流行的要属ASF了，更新活跃、强大的功能及跨平台可用，是它得到广泛使用的关键原因。这里我们采用ASF进行挂卡
ASF最新版本是3.x，相对于2.x需要依靠mono才能在非windows平台下运行，3.x提供了多个平台的编译程序，仅需安装对应系统的.NET Core即可使用（或者直接下载打包有对应系统.NET运行时的压缩包）

## 安装
根据[Setting-up](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up)所述，整个安装配置过程共分四步：
1. 安装.NTE Core依赖
2. 下载最新版本的ASF
3. 解压，并赋予执行权限（Linux/OS X）
4. 配置ASF

### 安装.NTE Core依赖
*大部分系统已自带依赖，可以选择跳过此步骤观察ASF是否正常运行来判断*
参考[prereqs.md](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)进行安装

值得注意的是Archlinux ARM自带的`libcurl.so.4`无法正常工作，会报错`System.DllNotFoundException: Unable to load DLL 'System.Net.Http.Native': The specified module or one of its dependencies could not be found.`[相关研究](https://steamcommunity.com/groups/ascfarm/discussions/1/1692659135916297867/)
解决方法是安装包`libcurl-compat`，并在最后使用`LD_PRELOAD=/usr/lib/libcurl.so.3 ./ArchiSteamFarm`运行ASF

### (可选)安装.NET Core
*下一步中若选择下载`ASF-generic.zip`则进行这一步*

所有系统的运行时都可以在[这里](https://github.com/dotnet/core-setup)找到，注意不要下错了
对于树莓派来说，我们需要的ARM架构运行时为[dotnet-runtime-latest-linux-arm.tar.gz](https://dotnetcli.blob.core.windows.net/dotnet/Runtime/release/2.0.0/dotnet-runtime-latest-linux-arm.tar.gz)
执行下列命令以安装：
```bash
wget https://dotnetcli.blob.core.windows.net/dotnet/Runtime/release/2.0.0/dotnet-runtime-latest-linux-arm.tar.gz
mkdir -p ~/dotnet && tar zxf ./dotnet-runtime-latest-linux-arm.tar.gz -C ~/dotnet
export PATH=$PATH:$HOME/dotnet
```
该命令会下载.NET运行时并解压至`~/dotnet`目录，同时在环境变量中添加安装路径
若想卸载删除`~/dotnet`目录并移除环境变量对应更改即可

### 下载ASF
下载[最新版本的ASF](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)
对于树莓派来说，一般选择下载`ASF-linux-arm.zip`，若下载`ASF-generic.zip`还需进行上一步提到的「[安装.NET Core](#anchor-3)」
下载没有什么需要注意的地方，不再赘述

### 解压并赋予执行权限
执行下列命令：
```bash
unzip ASF-linux-arm.zip -d asfv3 && cd asfv3
chmod +x ./ArchiSteamFarm
```

### 运行ASF
执行下列命令即可运行ASF：
```bash
./ArchiSteamFarm
```
对于Archlinux ARM须执行(「[安装.NTE Core依赖](#anchor-2)」步骤中提到):
```bash
LD_PRELOAD=/usr/lib/libcurl.so.3 ./ArchiSteamFarm
```
未出现报错即说明安装运行正常，进入下一步的配置

### 配置ASF
2.x的配置文件可直接覆盖使用
参考[Wiki](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)进行配置，或使用[在线生成器](https://justarchi.github.io/ArchiSteamFarm/#/)
英文苦手可参考[ASF 挂卡教程和命令集 - SteamCN 蒸汽动力](https://steamcn.com/t301016-1-1)

附我的配置文件：
```json
{
    "name": "journey_ad",
    "SteamLogin": "journey_ad",
    "SteamPassword": "******",
    "Enabled": true,
    "PasswordFormat": 0,
    "CustomGamePlayedWhileFarming": "我，博丽灵梦，自动接收龙狙中",
    "CustomGamePlayedWhileIdle": "女朋友",
    "TradingPreference": "AcceptDonations"
}
```
这套配置文件在挂卡时显示正在玩的游戏名为**我，博丽灵梦，自动接收龙狙中**，并可自动接受礼物；
挂卡完毕后显示正在玩的游戏名为**女朋友**

## 参考
[ASF V3.0 windows/linux含树莓派部署流程 - SteamCN 蒸汽动力](https://steamcn.com/t300500-1-1)
[ArchLinuxARM-armv7 - Unable to load DLL 'System.Net.Http.Native'](https://steamcommunity.com/groups/ascfarm/discussions/1/1692659135916297867/#c1692659135922168729)
[Configuration · JustArchi/ArchiSteamFarm Wiki](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)