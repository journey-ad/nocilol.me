---
title: 在linux下拨号上网(PPPoE)和开启无线热点
date: 2017-11-30 07:01:00
categories: lab
tags: [linux,拨号上网,PPPoE,无线热点]
urlname: dialing-internet--pppoe--and-opening-wireless-hot-spots-under-linux
---
[![测速结果](https://img.imjad.cn/images/2017/11/30/Screenshot-20171130162258-742x288.png "测速结果")](http://beta.speedtest.net/result/6827922547)
## 前言
权衡再三后，我最终选择了办理校园网，虽然有着种种问题，如绑定网卡MAC地址、限制终端数量(5个)、晚间断网、偶尔丢包严重等。但就目前来看，这依然是最优选择。我校校园网拨号方式与一般的PPPoE拨号方式并无不同，不需客户端，因此配置起来还算方便。
此外我的路由器留在了家里，而目前并无资金购置新的路由器。为了解决多个设备需要联网和没有路由器之间的矛盾，暂时使用无线网卡开启热点以供联网所用。
Windows下配置十分方便，Linux下则需要一些动手能力，以下是折腾记录：


## PPPoE拨号

### 安装
安装`rp-pppoe`
```bash
# pacman -s rp-pppoe
```

### 配置
执行配置程序，按提示填入用户名、密码、DNS、网络接口名、防火墙规则等。
网络接口名可通过执行`ifconfig`查询到
```bash
# pppoe-setup
```

### 拨号
配置完成之后即可开始拨号
```bash
# pppoe-start
```

### 断开连接
使用以下命令断开PPPoE连接
```bash
# pppoe-stop
```

### 开机自动拨号
添加到系统服务以实现自动拨号
```bash
# systemctl enable adsl
```

### 可能遇到的问题

#### 拨号成功后仍无法上网
执行```route```查看路由表，正常情况下返回结果中应有如下内容：
```bash
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         0.0.0.0         0.0.0.0         U     0      0        0 ppp0
```
没有的话手动添加
```bash
# route add default dev ppp0
```
一般情况下此时就可正常上网了。
使用以下脚本在PPPoE连接成功后自动添加路由表项：
```bash
#!/bin/sh
if ifconfig ppp0  > /dev/null 2>&1 ; then
    route del default
    route add default dev ppp0
fi
```
保存为`01-route.sh`，权限`root:root 755`，放至`/etc/ppp/ip-up.d`目录即可

## 开启无线热点

为了节省时间，简化配置，不重复造轮子，我们选择使用别人写好的工具[create_ap](https://github.com/oblique/create_ap)。为了感谢作者，你可以给他一颗🌟

### 安装脚本
[README.md](https://github.com/oblique/create_ap/blob/master/README.md)写的很清楚，不再赘述。
我的系统是[Arch Linux](https://www.archlinux.org/)，执行以下命令即可安装：
```bash
# pacman -S create_ap
```

### 开启
[README.md](https://github.com/oblique/create_ap/blob/master/README.md)中列举了几种经典的使用方法。大部分时间里，我需要的是[WPA + WPA2 passphrase](https://github.com/oblique/create_ap#wpa--wpa2-passphrase)
执行命令：
```bash
# create_ap wlan0 eth0 name password
```
`wlan0`和`eth0`替换为对应的网络接口名，同上，网络接口名可通过执行`ifconfig`查询到

### 作为服务启动
服务使用的配置文件路径是`/etc/create_ad.conf`，修改必要的值如`SSID`和`PASSPHRASE`
```bash
# systemctl start create_ap
```

### 开机自动开启热点
添加到系统服务以实现自动开启热点
```bash
# systemctl enable create_ap
```

## 参考内容
[在Linux上拨号上网(PPPoE)](https://vvl.me/2017/09/Linux-PPPoE/)
[Linux下pppoe连接建立后仍不能上网的问题](http://0x3f.org/post/problem-on-pppoe-connections-on-linux/)
[create_ap的GitHub项目页](https://github.com/oblique/create_ap)