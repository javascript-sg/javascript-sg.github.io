---
published: true
title: Making WordPress great again
layout: post
---
I recently upgraded to Ubuntu 16.04 only to learn that it's PHP7 now. I'm outdated. I wake up to finding my WordPress not working.

So we need to install php 7 and make some modifications.

```sh
sudo apt install php-fpm php-mysql
sudo apt install php7.0-xml
```

## Configuring PHP7

Edit your php config file

```sh
sudo nano /etc/php/7.0/fpm/php.ini
```

Change the `cgi.fix_pathinfo` setting to 0, you need to uncomment and change it.

```
cgi.fix_pathinfo=0
```

After doing so, you need to restart `php7.0-fpm`

```sh
sudo systemctl restart php7.0-fpm
```

## Configuring nginx

```
sudo nano /etc/nginx/nginx.conf
```

Add the following within the `http` block to point to your newly installed php7.0-fpm:

```
  upstream php {
    server unix:/run/php/php7.0-fpm.sock;
  }
```


This is my `global/wordpress.conf` designed to be placed inside the individual `server` blocks:

```
# WordPress single blog rules.
# Designed to be included in any server {} block.

rewrite /wp-admin$ $scheme://$host$uri/ permanent;

# Directives to send expires headers and turn off 404 error logging.
location ~* ^.+\.(ogg|ogv|svg|svgz|eot|otf|woff|mp4|ttf|rss|atom|jpg|jpeg|gif|png|ico|zip|tgz|gz|rar|bz2|doc|xls|exe|ppt|tar|mid|midi|wav|bmp|rtf)$ {
       access_log off; log_not_found off; expires max;
}

# Pass all .php files onto a php-fpm/php-fcgi server.
location ~ [^/]\.php(/|$) {
  if (!-f $document_root$fastcgi_script_name) {
    return 404;
  }
  include snippets/fastcgi-php.conf;
  fastcgi_pass php;
}
```