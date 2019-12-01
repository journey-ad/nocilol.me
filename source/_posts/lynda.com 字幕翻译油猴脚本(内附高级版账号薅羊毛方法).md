---
title: lynda.com 字幕翻译油猴脚本(内附高级版账号薅羊毛方法)
date: 2019-02-20 03:51:00
categories: code
tags: [学习,油猴]
urlname: lynda-subtitle-translation-script
---
![翻译效果](https://img.imjad.cn/images/2019/02/20/Xnip2019-02-20_15-01-53.jpg)



## 前言

lynda.com是一家美国的在线教育平台，课程内容涵盖多个方面

具体特色不作介绍，想要了解可以参考[亲身使用体验，白领的“职业技校”——Lynda.com](https://zhuanlan.zhihu.com/p/32590586)这篇文章



## 字幕翻译

greasyfork 地址：[lynda.com 字幕翻译](https://greasyfork.org/zh-CN/scripts/377991-lynda-com-%E5%AD%97%E5%B9%95%E7%BF%BB%E8%AF%91)



Lynda的字幕都是英文，对于英语能力不强的人来说，同步跟上视频进度有些压力

于是看了一下 lynda 的字幕实现，发现 hack 起来还挺容易的，就花了半天写了个脚本，调用翻译接口翻译字幕

翻译接口经过比较选择了搜狗，搜狗相对于其他翻译来说，翻译结果语法更通顺，且对于脏数据，如拼写错误、未加空格等情况适应性更好

因为是用户脚本，所以没有用公开的 API，而是扒了一下搜狗网页翻译的接口，签名算法简单分析一下 JS 就能找到

由于接口限制 5000 个字符，脚本中按 5000 字符分块异步请求翻译，然后在回调中拼合翻译结果，最后替换字幕文本



**第一次使用时会弹出跨域请求警告，这是搜狗网页翻译接口的限制，允许即可**

![跨域请求警告](https://img.imjad.cn/images/2019/02/20/Xnip2019-02-20_09-21-15.png)



需要注意的是，目前版本的 Tampermonkey(4.8) 在 Chrome 最新版(72.0.3626.109) 下工作异常，可选择降级 Chrome 为旧版本或暂时使用 [Tampermonkey BETA](https://chrome.google.com/webstore/detail/gcalenpjmijncebpfijmoaglllgpjagf)



## 薅羊毛

Lynda为订阅制，订阅价格不算便宜，对于个人用户来说：

| 月付                               | 年付                               |
| ---------------------------------- | ---------------------------------- |
| 免费试用1个月，<br/>每月29.99美元* | 免费试用1个月，<br/>每月24.99美元* |

下面介绍一个薅羊毛的方法，可以免费获取到Lynda的高级账户



### 步骤

1. 首先生成一个伪造的位于休斯顿的美国身份信息，使用[这个网站](https://www.fakeaddressgenerator.com/World_Address/get_us_address/city/Houston)生成，并建议保存下来
2. 在[休斯顿图书馆](https://halan.sdp.sirsi.net/client/en_US/hou/search/registration/$N?pc=SYMWS_HOUSTON)注册一张图书卡，以便获得休斯顿图书馆卡号
3. 使用步骤1中生成的身份信息填写注册表单
4. 注册完成后会获得一个临时ID，将其保存好，这就是Lynda的登录ID
5. 通过[这个链接](http://www.lynda.com/signin/organization)前往Lynda的组织登录
6. 组织信息填写「houstonlibrary.org」，然后新建一个配置文件，输入第4步得到的ID
7. Lynda会要求设置一个新密码，自行填写
8. 完成后Lynda会询问是否有旧帐户，点击「No i dont have any」
9. 由于是新帐户，系统会要求邮箱地址(此邮箱不作注册验证用，可以填写假邮箱或第1步里得到的随机邮箱)
10. 完成，可以无限制观看站内全部课程了

