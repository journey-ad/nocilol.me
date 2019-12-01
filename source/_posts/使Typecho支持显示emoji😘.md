---
title: 使Typecho支持显示emoji😘
date: 2016-05-06 18:04:00
categories: none
tags: []
urlname: make-typecho-support-display-emoji
---
![emoji][1]
原因：typecho默认使用utf-8编码，utf-8使用3个字节进行存储，而emoji有4个字节，因此将utf-8转换为utf8mb4即可解决
*注意：mysql的版本必须为v5.5.3或更高*
在数据库执行以下查询：
```sql
alter table typecho_comments convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_contents convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_fields convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_metas convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_options convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_relationships convert to character set utf8mb4 collate utf8mb4_unicode_ci;
alter table typecho_users convert to character set utf8mb4 collate utf8mb4_unicode_ci;
```
修改config.inc.php：
```php
$db->addServer(array (
  'host' => 'localhost',
  'user' => 'username',
  'password' => 'password',
  'charset' => 'utf8mb4',   //修改此行
  'port' => '3306',
  'database' => 'typecho',
), Typecho_Db::READ | Typecho_Db::WRITE);
```
大功告成?


  [1]: http://7xoffh.com1.z0.glb.clouddn.com/2016/05/336690927.png