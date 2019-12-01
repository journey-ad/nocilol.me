---
title: 为typecho后台增加登陆验证
date: 2015-10-24 04:01:00
categories: lab
tags: [typecho,php,登陆验证,原创]
urlname: to-increase-the-landing-validation-for-typecho
---
![shuibiao][1]
其实实现起来很简单，利用$_GET获取参数，加一个判断进行跳转就好了，你可以[点此][2]查看演示。
编辑admin文件夹下的login.php文件，在顶部加入如下代码：
```php
<?php
if($_GET["text1"]!="text2"){
header('Location: http://lovead.ml/fuck.php');//这里换成你的网址
exit;
}
?>
```
*（注意！今后若想登陆，需手动输入形同```http://lovead.ml/admin/login.php?text1=text2```的网址才能进入后台登录页面！）*

  [1]: http://7xoffh.com1.z0.glb.clouddn.com/2015/10/2998938332.jpg
  [2]: http://lovead.ml/admin