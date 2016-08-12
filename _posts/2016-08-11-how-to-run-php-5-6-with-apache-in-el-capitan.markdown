---
published: true
title: How to run PHP 5.6 with Apache in El Capitan
layout: post
---
I'm assuming you like to install PHP 5.6 with MySQL and want something more than MAMP. Good news is that Mac OS X El Capitan comes with Apache so you simply have to configure it.

To begin, you will need Homebrew, if you don’t already have that, install it [here](http://brew.sh/).

## Installing MySQL and PHP 5.6

```sh
brew install mysql
brew install php56
brew install php56-mcrypt
```

## Updating Apache httpd configuration

Next you will need to edit your Apache configuration. Open `/private/etc/apache2/httpd.conf for that`.

```sh
sudo nano /private/etc/apache2/httpd.conf
```

Search for `rewrite_module` and `php5_module`.

You have to uncomment them and change them to the following:

```perl
LoadModule rewrite_module libexec/apache2/mod_rewrite.so
LoadModule php5_module    /usr/local/opt/php56/libexec/apache2/libphp5.so
```

Notice that php5_module is referencing the Homebrew version. Note that your php.ini file can be found in: `/usr/local/etc/php/5.6/php.ini`

## Starting the servers

Start MySQL this way:

```sh
mysql.server start
```

Start Apache this way (you will need sudo):

```sh
sudo apachectl start
```

## Using VirtualHost

If you're using VirtualHost, you will need to uncomment the following line in `/private/etc/apache2/httpd.conf`. Search and uncomment this:

```perl
# Virtual hosts
Include /private/etc/apache2/extra/httpd-vhosts.conf
```

Next is to take a look at your VirtualHost file here `/private/etc/apache2/extra/httpd-vhosts.conf`:

```sh
sudo nano /private/etc/apache2/extra/httpd-vhosts.conf
```

This is a sample of my VirtualHost:

```
<VirtualHost cors.kahwee:80>
    DocumentRoot "/Users/kahwee/projects/cors"
    ServerName cors.kahwee
    ErrorLog "/private/var/log/apache2/cors.kahwee-error_log"
    CustomLog "/private/var/log/apache2/cors.kahwee-access_log" common
</VirtualHost>

<Directory /Users/kahwee/projects/>
  Options Indexes FollowSymLinks
  AllowOverride All
  Require all granted
</Directory>
```

Things to note in the above is the directory permissions. It may be different in your use case so be sure to set them properly. My project is using CakePHP is has the DocumentRoot in /Users/kahwee/projects/cors`