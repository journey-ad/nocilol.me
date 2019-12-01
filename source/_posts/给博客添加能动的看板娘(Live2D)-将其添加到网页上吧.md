---
title: 给博客添加能动的看板娘(Live2D)-将其添加到网页上吧
date: 2017-11-30 16:28:00
categories: lab
tags: [浮动小人,开源,Live2D,看板娘]
urlname: add-dynamic-poster-girl-with-live2d-to-your-blog-02
---
![药水制作师](https://img.imjad.cn/images/2017/11/20/PotionMaker2.png)
## 前言
在[上一篇文章](https://imjad.cn/archives/lab/add-dynamic-poster-girl-with-live2d-to-your-blog-01)里，我介绍了Live2D的一些技术背景和从手游「[药水制作师](https://play.google.com/store/apps/details?id=com.sinsiroad.potionmaker)」中提取模型的方法步骤。在这篇文章里，我将接着讲述如何利用WebGL SDK将其添加到网页上 ——就像本站一样。


> 更新：
[@保罗](https://paugram.com)写了一个typecho适用的插件，想要开箱即用的同学可以试试看啦~ [项目介绍](https://paugram.com/coding/add-poster-girl-with-plugin.html) !https://github.com/Dreamer-Paul/Pio
另外还有[@DaiDR](https://daidr.me)写的wordpress版 [项目介绍](https://daidr.me/archives/code-176.html)
<s style="font-size:10px;">房产证办理</s>[@小白-白](https://www.fczbl.vip/)写的wordpress版，模型是2233 [项目介绍](https://www.fczbl.vip/946.html)

{% cplayer %}
- 28138547
{% endcplayer %}

## 准备SDK
为了效率最大化，不用把有限的时间投入到与bug无限的斗争和踩坑中；为了不重复造轮子；为了爱与和平 <s>就是因为懒</s>，我们不使用官方的SDK，而是用[@EYHN](https://github.com/EYHN)大佬封装好的库来加载并显示模型。相比较[官方版本](http://sites.cybernoids.jp/cubism-sdk2_e/webgl2-1)而言，仅需一条命令即可进行模型的加载，无疑方便许多。

但对于「药水制作师」这款游戏的模型来说，上面所说的库还需经过一些修改。

修改原因
由于作者的特殊考量，#fea64e4没有实现对鼠标点击事件的处理，而是在鼠标移动时触发点击事件，我对此作了修改。
此外模型动作里有眨眼的动作，与默认的眨眼动作存在冲突，我注释了默认的眨眼动作
有趣的地方：默认的眨眼动作眼睛开合度给的上下限值为-1~1，但「药水制作师」的眼睛开合度为0.9以上时眼睛会消失
<s>少女盲目分析中</s>
![Pio眼睛的消失](https://img.imjad.cn/images/2017/11/20/sp171120_214419.png)
「药水制作师」带有一个久置动作，为了利用好这个动作，我通过监听`mouseout`增加了对其的支持。不用`mouseleave`的原因是Firefox对于`document`的`mouseleave`事件响应实现与Chrome不一致
同样，「药水制作师」带有数个点击动作，而模型中并不存在HIT_AREA，所以直接点击无效。我实现了通过`model.json`中提供的参数手动定义HIT_AREA。

基于[commit #fea64e4](https://github.com/EYHN/hexo-helper-live2d/commit/fea64e49a760ded5cc2dad974fd3d55bcebe15c6)修改，修改内容：

- 修复移动鼠标会触发点击事件的问题
- 增加鼠标点击事件
- 移除自带的眨眼动作
- 增加久置动作与事件支持
- 增加自定义HIT_AREA的方法
- 增加保存当前帧到文件的功能

由于原项目使用了GPL v2开源协议，修改后的代码已开源至[GitHub](https://github.com/journey-ad/live2d_src)，若想修改请参考[项目hexo-helper-live2d](https://github.com/EYHN/hexo-helper-live2d) <s>，对Node.js和JavaScript了解不深，功能实现的比较笨，希望有dalao pr🌝</s>

*鉴于该项目仍在活跃开发中，我修改的版本可能会随时间变化而过时*

**构建好的版本可以在[这里](https://github.com/journey-ad/live2d_src/releases)下载**!https://github.com/journey-ad/live2d_src/releases

## 提示功能（可选）
这部分直接抄自之前的浮动小人，做了部分修改，非必须项，须jQuery支持

若不启用此部分内容，下一步应作相应修改
此处代码不可直接套用，应根据自身情况进行修改

### 脚本
将以下内容保存为`waifu-tips.js`，放至相应目录
```javascript
function render(template, context) {

    var tokenReg = /(\\)?\{([^\{\}\\]+)(\\)?\}/g;

    return template.replace(tokenReg, function (word, slash1, token, slash2) {
        if (slash1 || slash2) {  
            return word.replace('\\', '');
        }

        var variables = token.replace(/\s/g, '').split('.');
        var currentObject = context;
        var i, length, variable;

        for (i = 0, length = variables.length; i < length; ++i) {
            variable = variables[i];
            currentObject = currentObject[variable];
            if (currentObject === undefined || currentObject === null) return '';
        }
        return currentObject;
    });
}
String.prototype.render = function (context) {
    return render(this, context);
};

var re = /x/;
console.log(re);
re.toString = function() {
    showMessage('哈哈，你打开了控制台，是想要看看我的秘密吗？', 5000);
    return '';
};

$(document).on('copy', function (){
    showMessage('你都复制了些什么呀，转载要记得加上出处哦', 5000);
});

$.ajax({
    cache: true,
    url: "path/to/waifu-tips.json",
    dataType: "json",
    success: function (result){
        $.each(result.mouseover, function (index, tips){
            $(document).on("mouseover", tips.selector, function (){
                var text = tips.text;
                if(Array.isArray(tips.text)) text = tips.text[Math.floor(Math.random() * tips.text.length + 1)-1];
                text = text.render({text: $(this).text()});
                showMessage(text, 3000);
            });
        });
        $.each(result.click, function (index, tips){
            $(document).on("click", tips.selector, function (){
                var text = tips.text;
                if(Array.isArray(tips.text)) text = tips.text[Math.floor(Math.random() * tips.text.length + 1)-1];
                text = text.render({text: $(this).text()});
                showMessage(text, 3000);
            });
        });
    }
});

(function (){
    var text;
    if(document.referrer !== ''){
        var referrer = document.createElement('a');
        referrer.href = document.referrer;
        text = 'Hello! 来自 <span style="color:#0099cc;">' + referrer.hostname + '</span> 的朋友';
        var domain = referrer.hostname.split('.')[1];
        if (domain == 'baidu') {
            text = 'Hello! 来自 百度搜索 的朋友<br>你是搜索 <span style="color:#0099cc;">' + referrer.search.split('&wd=')[1].split('&')[0] + '</span> 找到的我吗？';
        }else if (domain == 'so') {
            text = 'Hello! 来自 360搜索 的朋友<br>你是搜索 <span style="color:#0099cc;">' + referrer.search.split('&q=')[1].split('&')[0] + '</span> 找到的我吗？';
        }else if (domain == 'google') {
            text = 'Hello! 来自 谷歌搜索 的朋友<br>欢迎阅读<span style="color:#0099cc;">『' + document.title.split(' - ')[0] + '』</span>';
        }
    }else {
        if (window.location.href == 'https://imjad.cn/') { //如果是主页
            var now = (new Date()).getHours();
            if (now > 23 || now <= 5) {
                text = '你是夜猫子呀？这么晚还不睡觉，明天起的来嘛';
            } else if (now > 5 && now <= 7) {
                text = '早上好！一日之计在于晨，美好的一天就要开始了';
            } else if (now > 7 && now <= 11) {
                text = '上午好！工作顺利嘛，不要久坐，多起来走动走动哦！';
            } else if (now > 11 && now <= 14) {
                text = '中午了，工作了一个上午，现在是午餐时间！';
            } else if (now > 14 && now <= 17) {
                text = '午后很容易犯困呢，今天的运动目标完成了吗？';
            } else if (now > 17 && now <= 19) {
                text = '傍晚了！窗外夕阳的景色很美丽呢，最美不过夕阳红~';
            } else if (now > 19 && now <= 21) {
                text = '晚上好，今天过得怎么样？';
            } else if (now > 21 && now <= 23) {
                text = '已经这么晚了呀，早点休息吧，晚安~';
            } else {
                text = '嗨~ 快来逗我玩吧！';
            }
        }else {
            text = '欢迎阅读<span style="color:#0099cc;">『' + document.title.split(' - ')[0] + '』</span>';
        }
    }
    showMessage(text, 6000);
})();

window.setInterval(showHitokoto,30000);

function showHitokoto(){
    $.getJSON('https://api.imjad.cn/hitokoto/?cat=&charset=utf-8&length=28&encode=json',function(result){
        showMessage(result.hitokoto, 5000);
    });
}

function showMessage(text, timeout){
    if(Array.isArray(text)) text = text[Math.floor(Math.random() * text.length + 1)-1];
    console.log(text);
    $('.waifu-tips').stop();
    $('.waifu-tips').html(text).fadeTo(200, 1);
    if (timeout === null) timeout = 5000;
    hideMessage(timeout);
}
function hideMessage(timeout){
    $('.waifu-tips').stop().css('opacity',1);
    if (timeout === null) timeout = 5000;
    $('.waifu-tips').delay(timeout).fadeTo(200, 0);
}
```

### 文本
将以下内容保存为`waifu-tips.json`，放至相应目录
```javascript
{
    "mouseover": [
        {
            "selector": ".container a[href^='http']",
            "text": ["要看看 <span style=\"color:#0099cc;\">{text}</span> 么？"]
        },
        {
            "selector": ".fui-home",
            "text": ["点击前往首页，想回到上一页可以使用浏览器的后退功能哦"]
        },
        {
            "selector": "#tor_show",
            "text": ["翻页比较麻烦吗，点击可以显示这篇文章的目录呢"]
        },
        {
            "selector": "#comment_go,.fui-chat",
            "text": ["想要去评论些什么吗？"]
        },
        {
            "selector": "#night_mode",
            "text": ["深夜时要爱护眼睛呀"]
        },
        {
            "selector": "#qrcode",
            "text": ["手机扫一下就能继续看，很方便呢"]
        },
        {
            "selector": ".comment_reply",
            "text": ["要吐槽些什么呢"]
        },
        {
            "selector": "#back-to-top",
            "text": ["回到开始的地方吧"]
        },
        {
            "selector": "#author",
            "text": ["该怎么称呼你呢"]
        },
        {
            "selector": "#mail",
            "text": ["留下你的邮箱，不然就是无头像人士了"]
        },
        {
            "selector": "#url",
            "text": ["你的家在哪里呢，好让我去参观参观"]
        },
        {
            "selector": "#textarea",
            "text": ["认真填写哦，垃圾评论是禁止事项"]
        },
        {
            "selector": ".OwO-logo",
            "text": ["要插入一个表情吗"]
        },
        {
            "selector": "#csubmit",
            "text": ["要提交了吗，首次评论需要审核，请耐心等待~"]
        },
        {
            "selector": ".ImageBox",
            "text": ["点击图片可以放大呢"]
        },
        {
            "selector": "input[name=s]",
            "text": ["找不到想看的内容？搜索看看吧"]
        },
        {
            "selector": ".previous",
            "text": ["去上一页看看吧"]
        },
        {
            "selector": ".next",
            "text": ["去下一页看看吧"]
        },
        {
            "selector": ".dropdown-toggle",
            "text": ["这里是菜单"]
        },
        {
            "selector": "c-player a.play-icon",
            "text": ["想要听点音乐吗"]
        },
        {
            "selector": "c-player div.time",
            "text": ["在这里可以调整<span style=\"color:#0099cc;\">播放进度</span>呢"]
        },
        {
            "selector": "c-player div.volume",
            "text": ["在这里可以调整<span style=\"color:#0099cc;\">音量</span>呢"]
        },
        {
            "selector": "c-player div.list-button",
            "text": ["<span style=\"color:#0099cc;\">播放列表</span>里都有什么呢"]
        },
        {
            "selector": "c-player div.lyric-button",
            "text": ["有<span style=\"color:#0099cc;\">歌词</span>的话就能跟着一起唱呢"]
        },
        {
            "selector": ".waifu #live2d",
            "text": ["干嘛呢你，快把手拿开", "鼠…鼠标放错地方了！"]
        }
    ],
    "click": [
        {
            "selector": ".waifu #live2d",
            "text": ["是…是不小心碰到了吧", "萝莉控是什么呀", "你看到我的小熊了吗", "再摸的话我可要报警了！⌇●﹏●⌇", "110吗，这里有个变态一直在摸我(ó﹏ò｡)"]
        }
    ]
}
```

## 引入JS
修改`header.php`，加入以下内容以创建画布和提示框：
```html
<div class="waifu">
	<div class="waifu-tips"></div>
	<canvas id="live2d" width="280" height="250" class="live2d"></canvas>
</div>
```
在`footer.php`中加入以下内容：
```html
<script async src="path/to/waifu-tips.js"></script>
<script src="path/to/live2d.js"></script>
<script type="text/javascript">
	loadlive2d("live2d", "path/to/model.json");
</script>
```

上一篇文章提到的`model.json`修改为以下内容

其中`hit_areas_custom`字段的`head_x`和`body_x`定义了头部和身体的HIT_AREA的左上角的坐标，`head_y`和`body_y`定义了右下角的坐标

*坐标可通过启用`DEBUG_MOUSE_LOG`获取*
```javascript
{
	"version":"1.0.0",
	"model":"model.moc",
	"textures":[
		"textures/default-costume.png"
	],
	"layout":{
		"center_x":0.0,
		"center_y":-0.05,
		"width":2.0
	},
	"hit_areas_custom":{
		"head_x":[-0.35, 0.6],
		"head_y":[0.19, -0.2],
		"body_x":[-0.3, -0.25],
		"body_y":[0.3, -0.9]
	},
	"motions":{
		"idle":[
			{"file":"motions/WakeUp.mtn"},
			{"file":"motions/Breath1.mtn"},
			{"file":"motions/Breath2.mtn"},
			{"file":"motions/Breath3.mtn"},
			{"file":"motions/Breath5.mtn"},
			{"file":"motions/Breath7.mtn"},
			{"file":"motions/Breath8.mtn"}
		],
		"sleepy":[
			{"file":"motions/Sleeping.mtn"}
		],
		"flick_head":[
			{"file":"motions/Touch Dere1.mtn"},
			{"file":"motions/Touch Dere2.mtn"},
			{"file":"motions/Touch Dere3.mtn"},
			{"file":"motions/Touch Dere4.mtn"},
			{"file":"motions/Touch Dere5.mtn"},
			{"file":"motions/Touch Dere6.mtn"}
		],
		"tap_body":[
			{"file":"motions/Touch1.mtn"},
			{"file":"motions/Touch2.mtn"},
			{"file":"motions/Touch3.mtn"},
			{"file":"motions/Touch4.mtn"},
			{"file":"motions/Touch5.mtn"},
			{"file":"motions/Touch6.mtn"}
		],
		"":[
			{"file":"motions/Breath1.mtn"},
			{"file":"motions/Breath2.mtn"},
			{"file":"motions/Breath3.mtn"},
			{"file":"motions/Breath4.mtn"},
			{"file":"motions/Breath5.mtn"},
			{"file":"motions/Breath6.mtn"},
			{"file":"motions/Breath7.mtn"},
			{"file":"motions/Breath8.mtn"},
			{"file":"motions/Fail.mtn"},
			{"file":"motions/Sleeping.mtn"},
			{"file":"motions/Success.mtn"},
			{"file":"motions/Sukebei1.mtn"},
			{"file":"motions/Sukebei2.mtn"},
			{"file":"motions/Sukebei3.mtn"},
			{"file":"motions/Touch Dere1.mtn"},
			{"file":"motions/Touch Dere2.mtn"},
			{"file":"motions/Touch Dere3.mtn"},
			{"file":"motions/Touch Dere4.mtn"},
			{"file":"motions/Touch Dere5.mtn"},
			{"file":"motions/Touch Dere6.mtn"},
			{"file":"motions/Touch1.mtn"},
			{"file":"motions/Touch2.mtn"},
			{"file":"motions/Touch3.mtn"},
			{"file":"motions/Touch4.mtn"},
			{"file":"motions/Touch5.mtn"},
			{"file":"motions/Touch6.mtn"},
			{"file":"motions/WakeUp.mtn"}
		]
	}
}
```

## 增加样式
```css
.waifu {
    position: fixed;
    bottom: 0;
    left: 0;
    z-index: 1;
    font-size: 0;
    transition: all .3s ease-in-out;
    -webkit-transform: translateY(3px);
    transform: translateY(3px);
}
.waifu:hover {
    -webkit-transform: translateY(0);
    transform: translateY(0);
}
@media (max-width: 768px) {
    .waifu {
        display: none;
    }
}
.waifu-tips {
    opacity: 0;
    width: 250px;
    height: 70px;
    margin: -20px 20px;
    padding: 5px 10px;
    border: 1px solid rgba(224, 186, 140, 0.62);
    border-radius: 12px;
    background-color: rgba(236, 217, 188, 0.5);
    box-shadow: 0 3px 15px 2px rgba(191, 158, 118, 0.2);
    font-size: 12px;
    text-overflow: ellipsis;
    overflow: hidden;
    position: absolute;
    animation-delay: 5s;
    animation-duration: 50s;
    animation-iteration-count: infinite;
    animation-name: shake;
    animation-timing-function: ease-in-out;
}
.waifu #live2d{
    position: relative;
}

@keyframes shake {
    2% {
        transform: translate(0.5px, -1.5px) rotate(-0.5deg);
    }

    4% {
        transform: translate(0.5px, 1.5px) rotate(1.5deg);
    }

    6% {
        transform: translate(1.5px, 1.5px) rotate(1.5deg);
    }

    8% {
        transform: translate(2.5px, 1.5px) rotate(0.5deg);
    }

    10% {
        transform: translate(0.5px, 2.5px) rotate(0.5deg);
    }

    12% {
        transform: translate(1.5px, 1.5px) rotate(0.5deg);
    }

    14% {
        transform: translate(0.5px, 0.5px) rotate(0.5deg);
    }

    16% {
        transform: translate(-1.5px, -0.5px) rotate(1.5deg);
    }

    18% {
        transform: translate(0.5px, 0.5px) rotate(1.5deg);
    }

    20% {
        transform: translate(2.5px, 2.5px) rotate(1.5deg);
    }

    22% {
        transform: translate(0.5px, -1.5px) rotate(1.5deg);
    }

    24% {
        transform: translate(-1.5px, 1.5px) rotate(-0.5deg);
    }

    26% {
        transform: translate(1.5px, 0.5px) rotate(1.5deg);
    }

    28% {
        transform: translate(-0.5px, -0.5px) rotate(-0.5deg);
    }

    30% {
        transform: translate(1.5px, -0.5px) rotate(-0.5deg);
    }

    32% {
        transform: translate(2.5px, -1.5px) rotate(1.5deg);
    }

    34% {
        transform: translate(2.5px, 2.5px) rotate(-0.5deg);
    }

    36% {
        transform: translate(0.5px, -1.5px) rotate(0.5deg);
    }

    38% {
        transform: translate(2.5px, -0.5px) rotate(-0.5deg);
    }

    40% {
        transform: translate(-0.5px, 2.5px) rotate(0.5deg);
    }

    42% {
        transform: translate(-1.5px, 2.5px) rotate(0.5deg);
    }

    44% {
        transform: translate(-1.5px, 1.5px) rotate(0.5deg);
    }

    46% {
        transform: translate(1.5px, -0.5px) rotate(-0.5deg);
    }

    48% {
        transform: translate(2.5px, -0.5px) rotate(0.5deg);
    }

    50% {
        transform: translate(-1.5px, 1.5px) rotate(0.5deg);
    }

    52% {
        transform: translate(-0.5px, 1.5px) rotate(0.5deg);
    }

    54% {
        transform: translate(-1.5px, 1.5px) rotate(0.5deg);
    }

    56% {
        transform: translate(0.5px, 2.5px) rotate(1.5deg);
    }

    58% {
        transform: translate(2.5px, 2.5px) rotate(0.5deg);
    }

    60% {
        transform: translate(2.5px, -1.5px) rotate(1.5deg);
    }

    62% {
        transform: translate(-1.5px, 0.5px) rotate(1.5deg);
    }

    64% {
        transform: translate(-1.5px, 1.5px) rotate(1.5deg);
    }

    66% {
        transform: translate(0.5px, 2.5px) rotate(1.5deg);
    }

    68% {
        transform: translate(2.5px, -1.5px) rotate(1.5deg);
    }

    70% {
        transform: translate(2.5px, 2.5px) rotate(0.5deg);
    }

    72% {
        transform: translate(-0.5px, -1.5px) rotate(1.5deg);
    }

    74% {
        transform: translate(-1.5px, 2.5px) rotate(1.5deg);
    }

    76% {
        transform: translate(-1.5px, 2.5px) rotate(1.5deg);
    }

    78% {
        transform: translate(-1.5px, 2.5px) rotate(0.5deg);
    }

    80% {
        transform: translate(-1.5px, 0.5px) rotate(-0.5deg);
    }

    82% {
        transform: translate(-1.5px, 0.5px) rotate(-0.5deg);
    }

    84% {
        transform: translate(-0.5px, 0.5px) rotate(1.5deg);
    }

    86% {
        transform: translate(2.5px, 1.5px) rotate(0.5deg);
    }

    88% {
        transform: translate(-1.5px, 0.5px) rotate(1.5deg);
    }

    90% {
        transform: translate(-1.5px, -0.5px) rotate(-0.5deg);
    }

    92% {
        transform: translate(-1.5px, -1.5px) rotate(1.5deg);
    }

    94% {
        transform: translate(0.5px, 0.5px) rotate(-0.5deg);
    }

    96% {
        transform: translate(2.5px, -0.5px) rotate(-0.5deg);
    }

    98% {
        transform: translate(-1.5px, -1.5px) rotate(-0.5deg);
    }

    0%, 100% {
        transform: translate(0, 0) rotate(0);
    }
}
```

如一切正常，刷新网页后，可爱的Pio就会出现在页面左下角，点击会播放不同的动作并有相应提示文字

## 参考
[hexo-helper-live2d](https://github.com/EYHN/hexo-helper-live2d)
[live2d_src](https://github.com/journey-ad/live2d_src)