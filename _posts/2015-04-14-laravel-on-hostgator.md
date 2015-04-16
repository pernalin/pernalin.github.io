---
layout: post
title: Setting up Laravel on Hostgator
---

Have you developed a site using Laravel, and are looking to deploy it? One way to deploy it is to use a hosting 
service which is already configured for Laravel such as [fortrabbit](http://www.fortrabbit.com/) or [Laravel Forge](https://forge.laravel.com/) with [Digital Ocean](https://www.digitalocean.com/).

On the other hand, if you're like me and already have a hosting provider such as HostGator you may just wish to setup 
Laravel there. It's fairly simple. Below are the steps I followed to setup Laravel on my HostGator 
account.

* Connect to your HostGator terminal. 
I use PuTTY (http://www.putty.org/) to SSH into Hostgator. You'll need to find your Host 
Name or IP Address. Just log in to CPanel to find it.

PuTTY Configuration
Session:
- Host Name (or IP Address): The address you got above
- Port: Default is 22, but if you use a shared instance try 2222
- Connection type: SSH
Connection: Data
- Auto-login username: Your HostGator user name

I left the default settings for everything else.

* Check PHP version

Once connected, check PHP version of your hosted instance. You'll need to have at least PHP 5.2 to install Laravel.
```
#!cmd
username@domain [~]# php -v
```

* Install Composer
```
#!cmd
username@domain [~]# curl -sS https://getcomposer.org/installer | php
```

* Install Laravel
Install the right version of Laravel on your Host. If you don't know which version you have in Dev, use php artisan 
--version to find find out.

On the PuTTY terminal enter the following to install the latest Laravel version, or specify a particular version number.
 ```
#!cmd
username@domain [~]# composer create-project laravel/laravel --prefer-dist
OR
username@domain [~]# composer create-project laravel/laravel=4.2 --prefer-dist
```


* To remove a Laravel installation simply delete the Laravel folder
```
#!cmd
pernalin@diff-y.com [~]# rm -r laravel
```