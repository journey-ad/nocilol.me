---
title: 关于
date: 2014-08-23 18:37:16
type: about
---

<div class="about-avatar"></div>

> 黑夜一无所有
> 为何给我安慰
> 走在路上，放声歌唱
> 大风刮过山岗
> 上面是无边的天空
> 
> 　　　　黑夜的献诗


{% cplayer %}
- 405079779
- 1352292327
- 437250681
- 751472
- 425828206
- 1477670
- 478127
- 520459951
- 26220167
- 27548281
{% endcplayer %}
<style>
    .cplayer-template {
    margin-top: 50px;
    }
</style>

### **关于Blog**

博客从[**2014.08.11**](https://imjad.cn/archives/none/hello-world)至今已经运行<span id="htmer_time" style="color: #90CAF9; font-weight: bold;"></span>

本站使用开源静态网页生成工具[Hexo](https://hexo.io/)，主题使用[三斤](https://geek.lc/)提供的 Geek

托管于 [GitHub Pages](https://pages.github.com/)

由[Cloudflare](https://www.cloudflare.com/) 提供 DNS 解析服务，[Google Analytics](http://www.google.com/analytics/ "Google Analytics") 提供网站统计服务

- 2014.08.11建立，使用开源博客程序WordPress，托管于OpenShift
- 2015.07.22，更换为开源博客程序Typecho 1.0，托管于OpenShift
- 2015.12.12，迁移至腾讯云
- 2017.11.03，迁移至Azure新加坡
- 2019.12.01，迁移至Hexo，托管于GitHub Pages
- 2020.08.29，更换主题为 [Geek](https://github.com/journey-ad/hexo-theme-geek)


### **关于我**

九〇后，性别男，直男，爱好女。

有时文艺，有时严肃，有时逗比，怕生。

有时候看书，喜欢折腾一些东西。

现在是一个普通人。

INT~~P~~J，-0.7 0.6 -0.8

共产主义厨。

#### 你还可以在这些地方找到我
> **邮箱：** [&#97;&#100;&#64;&#105;&#109;j&#97;&#100;&#46;&#99;&#110;](mailto:ad&#64;&#105;&#109;&#106;&#97;&#100;&#46;&#99;&#110;)　　*//你的邮件会自动推送到我的手机上*
> **QQ：** [&#99;&#110;&#64;&#108;i&#110;&#117;&#120;&#46;&#99;&#111;m](http://sighttp.qq.com/authd?IDKEY=5ceb947ee4f6c45cacbadb41d4eeacc06466a1c469a71d94)　　*//请备注目的*
> **GitHub：** [journey-ad](https://github.com/journey-ad)　　*//欢迎Star，有Follow就更好了(☆ω☆)*
> **Steam：** [journey_ad](https://steamcommunity.com/id/journey_ad)　　*//喜+1*
> **Nintendo Switch Online：** SW-0794-3990-8205　　*//任天堂TMD就是世界的主宰！*
> **PSN(HK)：** [journey-ad](https://psnine.com/psnid/journey-ad)　　*//姨夫的微笑，就由我来守护！*
> …和其他同名ID

### **版权声明**

![知识共享许可协议](https://img.imjad.cn/images/2016/05/21/88x31.png)

博客内的所有原创内容（包括但不限于文章、图像等）除特别声明外均采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](http://creativecommons.org/licenses/by-nc-sa/4.0/)，任何人都可以自由传播，但不得用于商用且必须署名并以相同方式分享。

本站部分内容转载于网络，有出处的已在文中署名作者并附加原文链接，出处已不可寻的皆已标注来源于网络。若您认为本博客有部分内容侵犯了您的权益，请在评论区留言或电邮告知，我将认真处理。


### **支持我**

本博客为个人站点，无任何商业广告，若您想对我进行经济上的支持，请在下方选择付款方式，捐助所得资金将用于服务器/域名续费，我将对您表示感谢：)
<p class="donate-qrcode">
<img style="width: 25%;left: 25%;margin-right: 5px;display: inline;position: relative;" src="https://cdn.jsdelivr.net/gh/journey-ad/blog-img/about/alipay.jpg" alt="支付宝" title="支付宝"><img style="width: 25%;left: 25%;display: inline;position: relative;"src="https://cdn.jsdelivr.net/gh/journey-ad/blog-img/about/wechatpay.jpg" alt="微信支付" title="微信支付">
</p>

<script>
function secondToDate(second) {
     if (!second) {
         return 0;
     }
     var time = new Array(0, 0, 0, 0, 0);
     if (second >= 365 * 24 * 3600) {
        time[0] = parseInt(second / (365 * 24 * 3600));
        second %= 365 * 24 * 3600;
    }
    if (second >= 24 * 3600) {
        time[1] = parseInt(second / (24 * 3600));
        second %= 24 * 3600;
    }
    if (second >= 3600) {
        time[2] = parseInt(second / 3600);
        second %= 3600;
    }
    if (second >= 60) {
        time[3] = parseInt(second / 60);
        second %= 60;
    }
    if (second > 0) {
        time[4] = second;
    }
    return time;
};
function setTime() {
         // 博客创建时间秒数，时间格式中，月比较特殊，是从0开始的，所以想要显示5月，得写4才行，如下
         var create_time = Math.round(new Date(Date.UTC(2014, 7, 11, 18, 37, 16)).getTime() / 1000);// 当前时间秒数,增加时区的差异
         var timestamp = Math.round((new Date().getTime() + 8 * 60 * 60 * 1000) / 1000);
         currentTime = secondToDate((timestamp - create_time));
         if (currentTime[0]==0){
         	currentTimeHtml = currentTime[1] + '天'+ currentTime[2] + '时' + currentTime[3] + '分' + currentTime[4] + '秒';
         }else{
         	currentTimeHtml = currentTime[0] + '年' + currentTime[1] + '天' + currentTime[2] + '时' + currentTime[3] + '分' + currentTime[4] + '秒';
         }
		 // 兼容pjax，当htmer_time存在时输出，否则清空计时器
		 if (document.getElementById("htmer_time")){
			 document.getElementById("htmer_time").innerHTML = currentTimeHtml;
		 }else{
		 	 clearInterval(timer);
		 }
}
var timer = setInterval(setTime, 1000);
</script>