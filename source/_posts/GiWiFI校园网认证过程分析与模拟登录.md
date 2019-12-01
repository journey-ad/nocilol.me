---
title: GiWiFI校园网认证过程分析与模拟登录
date: 2018-08-29 02:50:00
categories: lab
tags: [Python,校园网,GiWiFi]
urlname: giwifi-auth-process-analysis-and-simulation-login
---
![マシュマロを食べさせあう、なでりん @ 相川りょう](https://img.imjad.cn/images/2018/08/29/67802201_p0.jpg)

{% cplayer %}
- 34003631
{% endcplayer %}

## 闲话

新学期开学之后，发现学校又不知从哪里引进了一套新的校园网络认证系统，叫做GiWiFi，有时候又被叫做GWiFi ~~GayWiFi~~

新的认证系统不同于之前的宽带拨号的形式，而是直接连接至网关设备，然后通过网页/客户端认证。但不知由于什么~~不可告人的~~原因，网页认证仅开放了半天，随后便只能下载客户端认证了

客户端引导下载页对于不同的设备和系统使用了UA进行区分，经过测试，提供的客户端有Windows、macOS、iOS以及Android版本（果然Linux又被忽视了，显示的是Android版的下载按钮🌝）

由于对此类认证客户端的排斥和心理洁癖，我便开始寻找使用网页认证，甚至是使用脚本模拟认证的方法，果不其然，经过一番分析与搜寻，我找到了一些东西


## 你藏得好深啊，登录框

上面说到网页认证开放半天后就被无情关闭，那么就先来找回消失的登录框吧~

此前的认证过程为
1. 打开任意http页面后被劫持至`172.17.1.1:8062/redirect`
2. 返回307跳转至认证页
3. 在认证页里输入账号密码登录认证
4. 关闭认证页，一段时间内即可正常上网

而现在跳转后仅显示客户端的下载按钮，直觉意识到这是一个专用于客户端引导下载的页面，真正的认证页仍另藏他处

事实正是如此，对比历史记录，现在的跳转页面的域名是`172.17.1.1`(校园网网关IP)，此前的认证页则是`http://login.gwifi.com.cn/cmps/admin.php/api/login/`

将客户端引导下载页的URL参数粘贴到认证页的URL后并打开，就能看到之前的认证页了（单纯打开首页会显示It works! ，不加任何参数打开的话则是跳转至新浪首页，这是哪位鬼才写的页面🌚）

认证页打开之后仍是一个大大的客户端下载按钮，但不要慌。打开审查元素就会发现，所有的登录框、重设密码框、注册框等都在，只是被隐藏掉了

去掉隐藏样式，正常输入账号密码登录即可，和之前的操作一模一样（登录后会跳转至百度首页，看来这两个页面是同一位鬼才写的🌝）

## 登录接口及参数分析

找到了登录框之后，就可以开始分析接口和参数了

直接看页面代码吧，写的挺乱的，好在未经过混淆，关键部分：

### 登录接口

```javascript
var loginAction  = function(params){
    var btn = $("#first_button");
    var round = Math.round(Math.random()*1000);
    var form = $("#frmLogin");
    $.ajax({
        url: "/cmps/admin.php/api/loginaction?round="+round,
        data: form.serialize(),
        type: "post",
        async: false,
        dataType: "json",
        success: function (data) {
            if (data.status === 1) {
                if(data.data.reasoncode == "44"){
                    params = getWechatParams(data);
                    //showSelectMessage(params);
                    wechatAuth(params.okParams);
                }else{
                    window.location.href = data.info;
                }
            } else {
                btn.removeAttr('disabled');
                doFailedLogin(data,"frmLogin");
                return false;
            }
        }
    });
}
```

可以看到接口为`/cmps/admin.php/api/loginaction`，参数都在登录表单里：

*关键部分已手动打码*

| 参数名        | 值                   | 说明                     |
| ------------- | -------------------- | ------------------------ |
| access_type   | 2                    | 作用未知                 |
| acsign        | ***                  | 登录状态接口中的sign字段 |
| btype         | pc                   | 猜测为平台类型               |
| client_mac    | ***                  | 客户端MAC                |
| contact_phone | 400-038-5858         | 服务电话                 |
| devicemode    |                      | 默认空值，作用未知       |
| gw_address    | 172.17.1.1           | 网关地址                 |
| gw_id         | ***                  | AP的SSID                 |
| gw_port       | 8060                 | 网关端口                 |
| lastaccessurl |                      | 默认空值，作用未知       |
| logout_reason | 0                    | 作用未知                 |
| mac           | ***                  | 同client_mac             |
| name          | ***                  | 账号                     |
| online_time   | 0                    | 猜测为在线时间，作用未知 |
| page_time     | 1535509645           | 登录页时间戳             |
| password      | ***                  | 密码                     |
| sign          | ***                  | 签名，可从登录表单中获取 |
| station_cloud | login.gwifi.com.cn   | 作用未知                 |
| station_sn    | ***                  | 猜测为基站ID             |
| suggest_phone | 400-038-5858         | 同contact_phone          |
| url           | http://www.baidu.com | 登录成功后跳转的网站     |
| user_agent    |                      | 默认空值，作用未知       |

观察后发现登录所需的大部分参数在认证页的URL参数里已经有了，剩下的有一部分已经在登录表单里填好了，另一部分需要从下文的登录状态接口中取到，将其组合起来后发送POST请求

登录成功后返回JSON数据：

```json
{
    "status":1,
    "info":"http://172.17.1.1:8060/wifidog/auth?token=***&info=***",
    "data":{
        "auth_verify":1,
        "reasoncode":0,
        "remain_time":1053640,
        "limit_time":null,
        "cost_type":4,
        "serviceplan_id":"1357",
        "is_share":"2",
        "wechat_enable":"1",
        "bw_up":"2048",
        "bw_down":"10240",
        "ontrial":0,
        "need_complete_data":null,
        "complete_data_url":null,
        "permit_intranet":2,
        "permit_internet":2,
        "carrier_operator":"3",
        "network_type":"2"
    }
}
```

其中的`info`字段的URL用作登录验证，使用GET请求就可以完成整个认证登录的流程了

登录失败的话`info`字段则会返回百度首页的URL，再次吐槽一下🌝

```json
{
    "status":1,
    "info":"http:\/\/www.baidu.com",
    "data":1
}
```


### 登录状态接口

```javascript
function initData(){
	//获取终端信息
	$.ajax({
        url: "http://172.17.1.1:8060/wifidog/get_auth_state?ip=***&mac=***&sign=***&callback=***",
        dataType:'jsonp',
        success: function(data) {
        	c = eval('(' + data.data + ')');
        	if(data.resultCode == 0){
        		fixData(c);
        	}else {
        		window.top.location.href = "http://www.baidu.com";
        	}
            return false;
        },
        error:function(data) {
        	return false;
        },
        cache: false
    });
}
```

同样可以看到接口为`/wifidog/get_auth_state`，参数为IP、MAC、签名和回调函数名，其中的签名可以直接在页面表单里取到

返回结果为JSONP数据，提取为：

```json
{
    "resultCode":0,
    "data":"{"auth_state":2,"gw_id":"***","access_type":"2","authStaType":"0","station_sn":"***","client_mac":"***","online_time":11,"logout_reason":7,"contact_phone":"400-038-5858","suggest_phone":"400-038-5858","station_cloud":"login.gwifi.com.cn","orgId":"899","sign":"***"}"
}
```

其中的`auth_state`字段值为`2`时为正常登录状态

观察登录成功后执行的操作，是替换了部分表单数据：

```javascript
function fixData(data) {
    $(".gw_id").val(data.gw_id);
    $(".access_type").val(data.access_type);
    $(".station_sn").val(data.station_sn);
    $(".client_mac").val(data.client_mac);
    $(".online_time").val(data.online_time);
    $(".logout_reason").val(data.logout_reason);
    $(".contact_phone").val(data.contact_phone);
    $(".suggest_phone").val(data.suggest_phone);
    $(".station_cloud").val(data.station_cloud);
    $(".acsign").val(data.sign);
}
```

进行模拟登录时也应一一替换

*需要注意的是参数`callback`是必需的，不然将不会返回`sign`字段值*


### 登出接口

登出功能在客户端引导下载页上，接口及参数为`http://172.17.1.1/getApp.htm?action=logout`

返回数据为：

```json
{
    "resultCode":0,
    "data":[]
}
```

其中`resultCode`字段值为`0`时登出成功


## 模拟登录脚本

现在登录相关接口和所需参数已经了解，可以开始写模拟登录了

这里使用Python来实现，理论上对所有GiWiFi系统通用

<script src="https://gist.github.com/journey-ad/73d900a675b090b9f2c94a1945e6ec19.js"></script>


## 参考

[校园网GiWiFi模拟登陆](https://www.bzqll.com/default/20171122-%E6%A0%A1%E5%9B%AD%E7%BD%91GiWiFi%E6%A8%A1%E6%8B%9F%E7%99%BB%E9%99%86.htm)