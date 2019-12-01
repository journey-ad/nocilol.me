---
title: 检测浏览器 DevTools 是否打开的几种方法
date: 2019-02-13 09:37:00
categories: lab
tags: [浏览器,DevTools]
urlname: check-browser-devtools-open
---
## 前言

今天关于看板娘的文章收到了一个[评论](https://imjad.cn/archives/lab/add-dynamic-poster-girl-with-live2d-to-your-blog-03/comment-page-1#comment-11331)

![Xnip2019-02-13_17-11-21.png](https://img.imjad.cn/images/2019/02/13/Xnip2019-02-13_17-11-21.png)

在此之前用金丝雀版本时也发现了这个情况，不过当时比较忙，又因为是金丝雀，离正式版还有一段距离，所以也就没放在心上🌝。如今用户提及，便不得不重视了

遂检查更新，发现Chrome正式版大版本号升级到了72，更新到新版本，果然出现了同样的问题

这个功能的核心在于「检测控制台是否打开，或打开时执行一段代码」，打开切图仔的好朋友——Stack Overflow搜索一番，发现已经有了一些解决方案。经过测试，问题可得以解决，在此整理下来留作记录



## 解决方案

### Chrome 72+可用

##### 利用Chrome开发者工具会自动读取元素id的特性，重写getter实现需求


```javascript
let element = document.createElement('checkDevTools');
Object.defineProperty(element, 'id', {
    get: function () {
        /* TODO */
        alert();
    }
});
console.debug(element); //使用console.debug()使打印出的辅助信息默认隐藏
```



##### 基本思路同上，使用img元素，结合`console.log()`的格式化输出，实现自定义打印内容

```javascript
let element = new Image();
Object.defineProperty(element, 'id', {
    get: function () {
        /* TODO */
        alert();
    }
});
console.log('%cCheck DevTools', element);
```



### Chrome 65+/Firefox可用

*这种方法只有在打开控制台时有效，即打开控制台后再访问页面无效，但每次打开控制台都会执行方法*

```javascript
var re = /x/;
re.toString = function () {
    /* TODO */
    alert();
    return '';
};
console.log(re);
```



### 其他方法

##### 通过检测窗口尺寸大小间接判断

优点是不受浏览器版本影响，同时缺点也很明显：打开控制台后再访问页面无效；控制台为独立窗口时无效；快速更改窗口大小、移动设备横竖屏切换可能会导致误报，需要作更多的兼容性判断

参考[sindresorhus/devtools-detect](https://github.com/sindresorhus/devtools-detect)

```javascript
/* eslint-disable spaced-comment */
/*!
	devtools-detect
	Detect if DevTools is open
	https://github.com/sindresorhus/devtools-detect
	by Sindre Sorhus
	MIT License
*/
(function () {
	'use strict';
	var devtools = {
		open: false,
		orientation: null
	};
	var threshold = 160;
	var emitEvent = function (state, orientation) {
		window.dispatchEvent(new CustomEvent('devtoolschange', {
			detail: {
				open: state,
				orientation: orientation
			}
		}));
	};

	setInterval(function () {
		var widthThreshold = window.outerWidth - window.innerWidth > threshold;
		var heightThreshold = window.outerHeight - window.innerHeight > threshold;
		var orientation = widthThreshold ? 'vertical' : 'horizontal';

		if (!(heightThreshold && widthThreshold) &&
      ((window.Firebug && window.Firebug.chrome && window.Firebug.chrome.isInitialized) || widthThreshold || heightThreshold)) {
			if (!devtools.open || devtools.orientation !== orientation) {
				emitEvent(true, orientation);
			}

			devtools.open = true;
			devtools.orientation = orientation;
		} else {
			if (devtools.open) {
				emitEvent(false, null);
			}

			devtools.open = false;
			devtools.orientation = null;
		}
	}, 500);

	if (typeof module !== 'undefined' && module.exports) {
		module.exports = devtools;
	} else {
		window.devtools = devtools;
	}
})();


// check if it's open
console.log('is DevTools open?', window.devtools.open);
// check it's orientation, null if not open
console.log('and DevTools orientation?', window.devtools.orientation);

// get notified when it's opened/closed or orientation changes
window.addEventListener('devtoolschange', function (e) {
    console.log('is DevTools open?', e.detail.open);
    console.log('and DevTools orientation?', e.detail.orientation);
});
```



此外还有一些旁门左道的方法，如通过控制台开启或关闭时执行基准代码消耗时长的区别来进行判断、使用一些浏览器私有的方法用作判断依据等。但这些方法或因性能问题，或由于失效，将不在此列出，有兴趣可查看Stack Overflow中该问题的历史回答



## 参考

[Find out whether Chrome console is open - Stack Overflow](https://stackoverflow.com/questions/7798748/find-out-whether-chrome-console-is-open)