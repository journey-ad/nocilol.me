---
title: Typecho开启伪静态并隐藏index.php
date: 2015-07-23 23:59:00
categories: code
tags: []
urlname: typecho-open-the-pseudo-static-and-hide-indexphp
---
Typecho后台设置永久链接后，会在域名后加上index.php，很多人都接受不了。例如如下网址：
```php
http://www.typechodev.com/index.php/archives/37/
```
但我们希望最终的形式是这样：
```php
http://www.typechodev.com/archives/37.html
```
那么我们如何做到这样的效果？

1.后台配置typecho伪静态
如图，在typecho后台，开启伪静态，并选择你喜好的url形式：
![184003255.png][1]

2.配置服务器的rewrite规则
如果在保存上述配置的时候，typecho无法自动配置，那么你可能需要手动配置服务器的rewrite规则。

nginx配置
```php
server {
        listen          80;
        server_name     yourdomain.com;
        root            /home/yourdomain/www/;
        index           index.html index.htm index.php;

        if (!-e $request_filename) {
            rewrite ^(.*)$ /index.php$1 last;
        }

        location ~ .*\.php(\/.*)*$ {
            include fastcgi.conf;
            fastcgi_pass  127.0.0.1:9000;
        }

        access_log logs/yourdomain.log combined;
    }
```
如果还有问题，请参考：http://docs.typecho.org/servers?s%5B%5D=nginx

apache配置
```php
<IfModule mod_rewrite.c>
    RewriteEngine On

    RewriteBase /

    RewriteCond %{REQUEST_FILENAME} !-f

    RewriteCond %{REQUEST_FILENAME} !-d

    RewriteRule ^(.*)$ index.php [L,E=PATH_INFO:$1]

</IfModule>
```
此配置可以放在apache的conf文件中，或者放在.htaccess中。

来自@TypechoDev.com


  [1]: http://7xoffh.com1.z0.glb.clouddn.com/2015/07/2728118486.png