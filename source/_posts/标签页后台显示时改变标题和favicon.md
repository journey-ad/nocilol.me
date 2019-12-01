---
title: 标签页后台显示时改变标题和favicon
date: 2015-10-24 18:37:00
categories: code
tags: [favicon]
urlname: change-title-and-favicon-when-the-tab-is-displayed
---
话不多说，先演示一下，效果如下
![demo][1]
*部分浏览器可能不支持动态favicon！*
新建js文件，输入下列代码，可结合自身情况进行调整。
改变标题：
```javascript
var OriginTitile = document.title;
var str;
document.addEventListener('visibilitychange', function() {
	if (document.hidden) {
		document.title = '( ͡° ͜ʖ ͡°)╯♂快去捡回来 - ' + OriginTitile;  //后台显示时的标题
		clearTimeout(str);
	} else {
		document.title = '( ͡° ͜ʖ ͡°)乖乖站好 - ' + OriginTitile;  //前台显示时的标题
		str = setTimeout(function() {
			document.title = OriginTitile;
		}, 2000);
	}
});
```

改变favicon：
```javascript
var changeIcon1 = function(){    
var link = document.head.querySelector("link");
link.href = "favicon1.gif";  //前台显示时的favicon，在网站根目录下
};
var changeIcon2 = function(){    
var link = document.head.querySelector("link");
link.href = "favicon2.gif";  //后台显示时的favicon，在网站根目录下
};
document.addEventListener('visibilitychange', function() {
	if (document.hidden) {
		changeIcon2();
	} else {
		changeIcon1();
	}
});
```

在header标签内引入上述js文件，即可看到效果。

*title部分代码来源于网络，出处已不可考，若有知道的烦请告诉我*
*站点加载完成后更换favicon的代码来源于[@琼台博客][2]*


  [1]: http://7xoffh.com1.z0.glb.clouddn.com/2015/10/2238202951.gif
  [2]: http://www.qttc.net/201308370.html