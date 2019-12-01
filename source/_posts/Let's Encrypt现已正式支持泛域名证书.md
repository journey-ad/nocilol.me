---
title: Let's Encrypt现已正式支持泛域名证书
date: 2018-03-15 05:39:00
categories: lab
tags: [证书,野卡,免费,Let's Encrypt]
urlname: letsencrypt-wildcard-certificate-support-is-live
---
![le-logo-wide.png](https://img.imjad.cn/images/2018/03/15/le-logo-wide.png)

## 前言
此前Let's Encrypt团队宣布将提供泛域名证书支持并于今年初开启了测试，经过几个月的测试(期间还跳票一次)，众人翘首以盼的“LE野卡”于昨天(2018.03.14)终于正式面向公众开放了。我也在第一时间申请了一张，以下是过程记录：


## 准备工作
Let's Encrypt的证书使用一个自动程序**Certbot**来完成申请验证发放等功能，但目前[官网](https://certbot.eff.org/)版本还未更新
因此我们从[GitHub](https://github.com/certbot/certbot)克隆一份最新开发版到本地：
```bash
cd ~
mkdir src && cd $_
git clone https://github.com/certbot/certbot.git
```
这条命令将在家目录`~`创建一个目录`src`并进入，然后克隆仓库`certbot/certbot`到本地

## 正式申请
泛域证书使用了**ACMEv2**，因此要通过参数手动指定API地址，执行命令：
```bash
cd certbot
./certbot-auto --server https://acme-v02.api.letsencrypt.org/directory -d imjad.cn -d *.imjad.cn --manual --preferred-challenges dns-01 certonly
```
这里使用了手动配置的方式，`-d`参数用于指定要申请的域名，一般须根域名和泛域名两个
在这里是`imjad.cn`和`*.imjad.cn`

执行后会先后要求提供邮箱地址、同意协议、同意公开服务器IP等
按要求填写和同意后，程序会要求解析增加一条TXT记录，按要求配置，回车进行验证
![le_txt.png](https://img.imjad.cn/images/2018/03/15/le_txt.png)

如一切正常，验证通过后证书就应发放成功了
![le_success.png](https://img.imjad.cn/images/2018/03/15/le_success.png)

接下来只需配置Web Server

## Nginx配置
泛域名证书与普通证书配置并无不同，甚至还更简单（多个Server块只需一套配置），这里放一个示例
```nginx
server {
        listen 443 ssl http2;
        server_name imjad.cn;
        root  /path/to/imjad.cn;
        index index.php index.html index.htm;
        ssl on;
        ssl_certificate      /etc/letsencrypt/live/imjad.cn-0001/fullchain.pem;
        ssl_certificate_key  /etc/letsencrypt/live/imjad.cn-0001/privkey.pem;
        ssl_dhparam /etc/ssl/dhparams.pem;
        ssl_session_cache shared:SSL:9m;
        ssl_session_cache shared:ssl_session_cache:10m;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;
        ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128
-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-R
SA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:D
HE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!3DES:!MD5:!PSK';
        ssl_stapling on;
        ssl_stapling_verify on;

        location ~ .*\.php$
        {
            fastcgi_pass  unix:/tmp/php-cgi-imjad.cn.sock;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
}
```
配置完成后，执行`sudo nginx -s reload`重载Nginx配置

刷新网页，应该就能看到新的证书了
![le_cert.png](https://img.imjad.cn/images/2018/03/15/le_cert.png)

## 证书续期
*目前尚未清楚证书是否可以自动续期，此处待编辑*

## 小结
泛域名证书相对单域名或多域名证书带来的好处是显而易见的。有再多子域名也不必担心，极大方便了配置
感谢Let's Encrypt项目和团队，他们对互联网安全的贡献必将载入互联网发展史册