---
title: RUA！真实终端大秦话
date: 2017-10-23 12:03:00
categories: lab
tags: []
urlname: rua-true-terminal-quin-lang
---
![rua screenshot](https://img.imjad.cn/images/2017/10/23/Screenshot-20171023145754-984x592.png)

[项目地址](https://github.com/journey-ad/quin)
<gb>journey-ad/quin</gb>

**quin** 是一个简单的 motd 脚本，内容基于一种由大秦后人，“现在在斗鱼TV<del>微博</del>上班，每天都会摸鱼” 的主播 [Mr.Quin][quin] 所建立的大秦话。它会输出一些随机的大秦话，其中有时会穿插基于你计算机上的东西。

截图中使用的 shell 为 [FISH][fish-shell]

## 特性

* 随机放置和着色的随机字符串，组成一段大秦话
* 帮助各位 new ass 时刻学习大秦话
* 获取系统数据，如主机名、正在运行的进程、当前用户和 `$EDITOR`
* 如果你有 [lolcat][lolcat]，你可以这样做：
  `while true; do quin | lolcat -a -d 100 -s 100 -p 1; done`
  (感谢 [hom3chuk][hom3chuk])
* 支持管道连接： `ls /usr/bin | quin` 将打印在 /usr/bin 中找到的一些可执行文件
* 还有一些其他的参数开关，详情请看 `quin -h`
* RUA！

## 安装

`pip install quin`

如果你因为某种原因不想这么干, 执行 `python setup.py install` 也是可以的

请注意，如果你是不具有 argparse (<=Python2.6) 环境的 new ass， 你必须从 pypi 手动安装 argparse

然后只需把 `quin` 添加到你所用 shell 的 rc 文件底部即可

## 提示

你需要在支持 unicode 的系统上运行支持256色的终端

秦先生的头像是用 Gimp 和 [img2xterm][i2x] 创建的

[quin]: https://zh.moegirl.org/Mr.Quin
[i2x]: https://github.com/rossy2401/img2xterm
[hom3chuk]: https://github.com/hom3chuk
[lolcat]: https://github.com/busyloop/lolcat
[fish-shell]: https://github.com/fish-shell/fish-shell