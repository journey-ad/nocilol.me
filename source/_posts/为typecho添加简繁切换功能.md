---
title: 为typecho添加简繁切换功能
date: 2015-07-25 00:33:00
categories: code
tags: [简体,繁体,typecho,javascript,转载]
urlname: add-simplified-switching-function-for-typecho
---
其实这个方法是通用的，跟用的什么程序没有太大的关系，之前在WP上我也是用这个方法。

很久之前我曾想做3种语言的博客，后来一想，还是中文吧，英文的朋友毕竟很少接触，简体、繁体切换就简单的多了，没有采用2个译文的办法，只使用了js，其实简体和繁体在语言上有很大的差异，js只能做到字形的转换，语言的使用，例如语法还是大陆的语法，港澳台的朋友凑合着看吧。各位繁体控也可以切换到繁体。。。（ps.本人喜欢打简体、看繁体）这样数据库中存的都是简体，外界喜欢简体的就简体，喜欢繁体的就繁体。好吧，我就不废话了。
首先下载js文件，将下面代码中的网址改成自己的。
```javascript
var defaultEncoding = 2; //1: Tranditional Chinese, 2: Simplified Chinese
var translateDelay = 0;
var cookieDomain = "https://imjad.cn/"; //改为你自己的域名
var msgToTraditionalChinese = "繁體";  //此处可以更改为你想要显示的文字
var msgToSimplifiedChinese = "简体";  //同上
var translateButtonId = "translateLink";
```
footer.php中添加以下代码
```php
<!--- 简繁转换开始 --->
<script type="text/javascript" src="<?php $this->options->themeUrl(); ?>js/cn_tw.js"></script> // 此处地址改为你的js文件地址
<script type="text/javascript">
var defaultEncoding = 0; //默认是否繁体，0-简体，1-繁体
var translateDelay = 0; //延迟时间,若不在</body>前, 要设定延迟翻译时间, 如100表示100ms,默认为0
var cookieDomain = "https://imjad.cn/";    //Cookie地址, 一定要设定, 通常为你的网址
var msgToTraditionalChinese = "繁體"; //默认切换为繁体时显示的中文字符
var msgToSimplifiedChinese = "简体"; //默认切换为简体时显示的中文字符
var translateButtonId = "translateLink"; //默认互换id
translateInitilization();
</script>
<!--- 简繁转换结束 --->
```
修改网址为自己的。。。不要老是让我提醒嘛。。。
在你想要添加转换按钮的地方添加以下代码
```php
<a id="translateLink">繁體</a>
```
文字可以按照自己的喜好更改。如果放在格式代码中，请加相应的格式。
js下载地址：[cn_tw.js.zip][1]

本文来自[@萧篱落][2]，有部分删改。


  [1]: https://imjad.cn/cn_tw.js.zip
  [2]: http://xiaoliluo.com/63.html