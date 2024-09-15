---
id: 33
title: WordPress
date: '2024-05-12T10:01:56+00:00'
author: Jon
layout: post
guid: 'http://jonblog.site/?p=33'
permalink: /2024/05/12/wordpress/
categories:
    - TOOL
---



# Install guide

https://www.linuxtuto.com/how-to-install-wordpress-on-debian-12/

Try not to create a /etc/nginx/conf.d/wordpress.conf file; instead, make the necessary configurations directly in `/etc/nginx/sites-enabled/default` .

装完这个后， 之前按照这个安装,

https://shape.host/resources/certbot-installation-guide-on-debian-12

```shell
sudo apt update
sudo apt upgrade
sudo apt install certbot python3-certbot-nginx

sudo certbot --apache -d jonblog.site -d www.jonblog.site
certbot certificates
systemctl status certbot.timer
certbot renew --dry-run
```

如果还是不行，把`/etc/nginx/sites-available/default`也配置上。

/var/www/html/wordpress/wp-config.php

目前这个没有配置

```
/**
define('FORCE_SSL_ADMIN', true);

if (strpos($_SERVER['HTTP_X_FORWARDED_PROTO'], 'https') !== false) {
    $_SERVER['HTTPS'] = 'on';
}

*/
```

set   https://jonblog.site for
WordPress Address (URL)    
Site Address (URL)

# wordpress backup

https://www.youtube.com/watch?v=2qoX-siH0cY