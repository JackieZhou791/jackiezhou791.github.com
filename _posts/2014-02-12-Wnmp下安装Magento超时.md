---
layout: post
title: Wnmp下安装Magento超时 
---

Wnmp下安装Magento超时问题

由于Wnmp中Nginx参数fastcgi_connect_timeout默认为60s

请将以下配置下加入nginx.conf或者vhost conf中

```
fastcgi_send_timeout 300;
fastcgi_read_timeout 300;
fastcgi_connect_timeout 300;
```

并修改php.ini中的max_excution_time

```
max_excution_time = 300
```

Thanks!
