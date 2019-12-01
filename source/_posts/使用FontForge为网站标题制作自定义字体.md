---
title: 使用FontForge为网站标题制作自定义字体
date: 2018-08-01 05:18:00
categories: lab
tags: []
urlname: make-a-custom-font-for-the-title-of-the-website
---
![CAT @ Mr.Goblin](https://img.imjad.cn/images/2018/08/01/66684180_p0.jpg)


{% cplayer %}
- 5042046
{% endcplayer %}

## 前言
近日与往常一样在网上闲逛时，发现[@Trii Hsia](https://yumoe.com)的博客用了一款非常活泼的英文字体Josefin Sans，于是便抄了过来。换上之后整个博客都感觉活跃了起来，唯一不满的是博客标题用的还是一般的黑体，显得过于庄重了，就想着能找一款与其搭配的中文字体，最终选中了もじワク研究的[マキナス Scrap 5](https://moji-waku.com/makinas/index.html)

最终结果也较为理想，你可以在本博客首页的标题和页脚区域上看到效果，或是
<span class="cat-and-sunflower" style="display: block;margin: 0 auto;text-align: center;font-size:3em">猫 与 向 日 葵</span>*是可以选中和复制的文字，不是图片哦~*

## 中文字体的困境
此前我曾在「[关于如何在网页中引入思源字体：漫谈Typekit](https://imjad.cn/archives/lab/how-to-introduce-source-han-fonts-into-web-pages-through-typekit)」中提到过，中文字体在网页上的应用一直是个难题。首当其冲的就是中文字体的恐怖容量，不同于西文字体只有数十个符号，一套包含简繁体的中文字体大约包含了2~3万个符号，设计流程须2~4年之久，这不是一般的个人或小公司能够负担的起的，所以一般我们看到的中文字体，多是由大公司制作，成本巨大致使商用代价也是十分高昂。包含的符号众多也导致了中文字体动辄4~5M的大小。桌面应用尚且可行，对于Web来说是无论如何都不能接受的

本地引用的弊端促使了WebFont的流行，但碍于版权问题，一般只有寥寥几种中文字体可选，对于想用一些比较「偏门」的字体的人，只能选择在本地引入了。而选择本地引用，就又回到了上面说的问题，这也是我们在网页上鲜少看到多彩的中文字体的原因之一

如果换个思路，一个网页一般情况下只使用了一部分字符，那么我们把所需的这些个字符从原始字体中提取出来，然后组成为一个新字体，不就可以减少大小了吗？Typekit正是这样做的，切片按需加载字体，极大减少了资源浪费。按着同样的思路，经过一番搜索之后，我找到了提取字体的方法

>关于中文字体，还有一些其他的压缩工具，比如[字蛛](http://font-spider.org/)，但它是针对静态的html页设计的，还需要node环境，相对来说更适合静态博客
>我在文中介绍的方法由于是手动操作，效率并不高，只适合制作特定的几个字符，如网站标题或LOGO等

## 自己「造」一个吧
*首先声明，这种方法在版权上是有问题的，毕竟没有哪家字体制作者会让你二次修改字体数据，所以本文仅做方法的介绍*
经过搜寻，我找到了提取并制作字体的工具——大名鼎鼎的FontForge

### 下载并安装FontForge
在[FontForge](https://github.com/fontforge/fontforge)的发布页下载安装程序，最新版本是[2017.07.31](https://github.com/fontforge/fontforge/releases/tag/20170731)释出的，下载和安装过程不表

## 提取需要的字符
启动FontForge，然后找到待提取的字体，选择打开

初次打开可能会提示缺失Cidmap文件，在[这里](https://fontforge.github.io/cidmaps.tgz)下载并解压，按提示加载即可

![Cidmaps缺失](https://img.imjad.cn/images/2018/08/01/sp180801_134430.png)

![打开字体](https://img.imjad.cn/images/2018/08/01/sp180801_134359.png)

![打开后](https://img.imjad.cn/images/2018/08/01/sp180801_134802.png)

### 修改首选项
依次点击**File - Preferences**进入首选项设置，然后点击左侧的**New Font**选项卡，将**NewCharset**更改为`ISO 10646-1 (Unicode, Full)`，这样在新建字体时就会自动使用完整Unicode字符的代码页

![更改代码页](https://img.imjad.cn/images/2018/08/01/sp180801_134851.png)

### 新建空白字体
依次点击**File - New**新建一个空白字体，并点击**Element - Font Info**配置字体信息

注意此处的`Family Name`字段，应与最终在网页上调用的`font-family`属性值相匹配

![配置字体信息](https://img.imjad.cn/images/2018/08/01/sp180801_141000.png)

### 复制所需字符
回到原始字体窗口，选择所需字符后点击右键，在右键菜单中选择**Copy**或是按<kbd>Ctrl</kbd>+<kbd>C</kbd>以复制，并在新建的字体窗口中点击右键选择粘贴，同样，<kbd>Ctrl</kbd>+<kbd>V</kbd>也是可以的
*注意点击并拖动可快速选中多个字符，另外对于不同的代码页，字符顺序和有无情况是不一致的，需仔细观察确定*

排在前边的数字和字母可以直接拖选复制，对于汉字等排在后边的字符，一个个翻肯定是行不通的，这时就要找到字符对应的Unicode编码，然后点击**View - Goto**直接跳转过去

![跳转](https://img.imjad.cn/images/2018/08/01/sp180801_141042.png)

Unicode编码转换可以使用网上的在线服务，注意要以开头为`u+`的形式

我写了一个方法
```javascript
function encodeUnicode(str) {
    var data = {};
    for (var i=0;i<str.length;i++) {
        data[str[i]] = 'u+' + ('00' + str.charCodeAt(i).toString(16)).slice(-4);
    }
    console.table(data);
}

encodeUnicode('猫与向日葵')
```
在控制台执行即可

![Unicode编码转换](https://img.imjad.cn/images/2018/08/01/sp180801_144631.png)

### 生成新字体
依次点击**File - Generate Fonts**，然后选择所需格式生成，生成时会出现一些警告和错误，可以忽略

![生成字体](https://img.imjad.cn/images/2018/08/01/sp180801_140709.png)

可以看到，提取之后的字体大小仅有数KiB，相对于原始字体的大小来说已经十分可观了，而显示指定字符却与原始字体相比没有任何区别

![字体大小对比](https://img.imjad.cn/images/2018/08/01/sp180801_140801.png)

## 应用到网页上
CSS中的`@font-face`规则可以用来引入自定义字体([MDN文档](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face))
具体语法不再赘述，最终结果为：
```css
@font-face {
    font-family: 'Makinas-Scrap-5-Cat-and-Sunflower';
    src:
      url('//cdn.imjad.cn/usr/themes/Moricolor/assets/Makinas-Scrap-5-Cat-and-Sunflower/Makinas-Scrap-5-Cat-and-Sunflower.svg?v1.000') format('svg'),
      url('//cdn.imjad.cn/usr/themes/Moricolor/assets/Makinas-Scrap-5-Cat-and-Sunflower/Makinas-Scrap-5-Cat-and-Sunflower.otf?v1.000') format('otf'),
      url('//cdn.imjad.cn/usr/themes/Moricolor/assets/Makinas-Scrap-5-Cat-and-Sunflower/Makinas-Scrap-5-Cat-and-Sunflower.ttf?v1.000') format('ttf'),
      url('//cdn.imjad.cn/usr/themes/Moricolor/assets/Makinas-Scrap-5-Cat-and-Sunflower/Makinas-Scrap-5-Cat-and-Sunflower.woff?v1.000') format('woff');
    font-weight: normal;
    font-style: normal;
}
```

然后将所需元素的`font-family`样式指定为`Makinas-Scrap-5-Cat-and-Sunflower`即可生效

## 参考
[fontforge制作自定义字体及在手机上应用举例](https://www.zhangxinxu.com/wordpress/2011/11/fontforge%E8%87%AA%E5%AE%9A%E4%B9%89%E6%89%8B%E6%9C%BA%E5%AD%97%E4%BD%93/)

[@font-face - CSS: Cascading Style Sheets](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face)