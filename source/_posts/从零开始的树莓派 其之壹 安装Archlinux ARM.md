---
title: 从零开始的树莓派 其之壹 安装Archlinux ARM
date: 2017-07-29 17:06:00
categories: lab
tags: [linux,树莓派,archlinux,硬件,实验室]
urlname: my-little-raspberry-pi-01
---
![RPi3b_neofetch](https://img.imjad.cn/images/2017/07/30/Screenshot-20170730004921-824x478.png "RPi3b_neofetch")
老早就在关注树莓派这个小东西了，但是没有想到合适的用途所以就一直在观望<span style="font-size:10px">其实是因为没有钱</span>刚好前几天Diy的群里一位小伙伴要出一台3b，就借着这个机会入手了


{% cplayer %}
- 28283602
{% endcplayer %}

## 树莓派3B简介
>The Raspberry Pi 3 is the third-generation Raspberry Pi. It replaced the Raspberry Pi 2 Model B in February 2016.
>- Quad Core 1.2GHz Broadcom BCM2837 64bit CPU
>- 1GB RAM
>- BCM43438 wireless LAN and Bluetooth Low Energy (BLE) on board
>- 40-pin extended GPIO
>- 4 USB 2 ports
>- 4 Pole stereo output and composite video port
>- Full size HDMI
>- CSI camera port for connecting a Raspberry Pi camera
>- DSI display port for connecting a Raspberry Pi touchscreen display
>- Micro SD port for loading your operating system and storing data
>- Upgraded switched Micro USB power source up to 2.5A

可以看到，3b的内存是1GiB，同时自带了无线和蓝牙模块，可以为我们省下两个USB端口

### 所需外设/配件
- 一张Micro SD卡 \* //推荐32GiB以上，Class 10级别
- 至少5V/2A的电源适配器和Micro USB电源线\* //官方推荐的输出电流是2.5A，实际使用中2A即可满足，若你的外设较多请酌情升级
- Micro SD卡读卡器 \*
- 显示器，HDMI或GPIO均可
- 键鼠
- 以太网线
- 电池

## 写入系统镜像
作为Arch Linux用户，我选择的是和[Arch Linux](https://archlinux.org/ "Arch Linux")体验基本一致的[Arch Linux ARM](https://archlinuxarm.org/ "Arch Linux ARM")系统，其他用户可以选择使用树莓派基金会官方支持的[Raspbian](https://www.raspberrypi.org/downloads/raspbian/ "Raspbian")系统。或者使用更适合初学者的[NOOBS](https://www.raspberrypi.org/downloads/noobs/ "NOOBS")

按照[官方文档](https://archlinuxarm.org/platforms/armv8/broadcom/raspberry-pi-3#installation "官方文档")进行操作，大概步骤是新建两个分区，一个作为```boot```分区，一个作为```/```，将下载好的镜像解压至```/```，并将```/boot```下的文件移动至```boot```分区，过程还是很简单的

## 安装并配置系统
**接下来的操作将在树莓派上进行**

将Micro SD卡插入树莓派相应位置，插入Micro USB电源线**（树莓派没有设计开关，插入电源即为开机状态）**。我并未使用显示器，为方便后续操作，我同时插入了以太网线

### 联网
接入以太网的树莓派应当能自动连接到网络（需要路由器等设备开启```dhcp```服务），未能连接成功的用户可参考[此文档](https://wiki.archlinux.org/index.php/Network_configuration "此文档")

稍等片刻后，即可通过```ssh```命令登录到路由器

### SSH登录
>Login as the default user ```alarm``` with the password ```alarm```.
>The default ```root``` password is ```root```.

>可选操作：
>
>默认的用户名和密码均为```alarm```，欲更改可参照[此文档](https://wiki.archlinux.org/index.php/Users_and_groups "此文档")
>
>默认用户没有```sudo```权限，按照[此文档](https://wiki.archlinux.org/index.php/Sudo "此文档")添加权限
>
>更换```pacman```镜像源为[清华大学tuna源](https://mirrors.tuna.tsinghua.edu.cn/help/archlinux/ "清华大学tuna源")
>添加```archlinuxcn```仓库的[tuna源](https://mirrors.tuna.tsinghua.edu.cn/help/archlinuxcn/ "tuna源")

#### 首先更新系统：
```shell
# pacman -Syyu
```
此命令将刷新软件包缓存并更新系统


#### 可安装桌面环境，我装的是```Xfce```：
```shell
# pacman -S xorg xfce4
```
我在图形界面的操作均在VNC下进行，因此不需要登录管理器


#### 安装```yaourt```：
AUR是Arch Linux的一大特色，在Arch Linux ARM上不能直接安装，使用下列方法

首先安装```base-devel```这个包，这个包包含了编译所需的工具

```shell
# pacman -S base-devel
```
安装```packge-query```

```shell
$ wget https://aur.archlinux.org/packages/pa/package-query/package-query.tar.gz
$ tar zxvf package-query.tar.gz
$ cd package-query
$ makepkg -si
```

安装```yaourt```

```shell
# pacman -S yaourt
```


## 完成
至此系统已基本达到可用状态，更多配置留待下一篇文章再谈