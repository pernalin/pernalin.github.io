---
layout: post
title: Installing Laravel on Hostgator
---

Have you developed a site using Laravel, and are now ready to deploy it? One way is to use a 
hosting service already configured for Laravel such as [fortrabbit](http://www.fortrabbit.com/) or [Laravel Forge](https://forge.laravel.com/) with [Digital Ocean](https://www.digitalocean.com/).

On the other hand, if you're like me and already have a hosting provider such as HostGator you may just wish to setup 
Laravel there. It's fairly simple. Below are the steps I followed to setup Laravel on my HostGator 
account.

### Connect to your HostGator terminal. 
I use [PuTTY](http://www.putty.org/) to SSH into Hostgator. You'll need to have your Host 
Name or IP Address ready. If you don't have this already, log in to HostGator's CPanel to find it.

PuTTY Configuration  
*    Session:  
     > Host Name (or IP Address): Your domain name, or IP    
     > Port: Default is 22, but if you use a shared instance try 2222    
     > Connection type: SSH  
*    Connection: Data  
     > Auto-login username: Your HostGator user name  

I left the default settings for everything else. If the above doesn't work and you need more info check out [this 
link](https://support.hostgator.com/articles/specialized-help/technical/ssh-keying-through-putty-on-windows-or-linux)

### Check PHP version

Once connected, check PHP version of your hosted instance. You'll need to have at least PHP 5.2 to install Laravel.

{% highlight console darkbg linos %}
username@domain [~]# php -v
PHP 5.4.38 (cli) (built: Mar 11 2015 12:43:38)
{% endhighlight %}  

### Install Composer
While you don't have to do this, i.e. you could simply copy your local Laravel folder, having Composer makes it much 
easier to install Laravel and any related packages.

{% highlight console %}
username@domain [~]# curl -sS https://getcomposer.org/installer | php
All settings correct for using Composer
Downloading...
Composer successfully installed to: /home1/pernalin/composer.phar
Use it: php composer.phar
{% endhighlight %}  

### Install Laravel
Install the right version of Laravel on your Host. If you don't know which version you have in Dev, use `php artisan 
--version` to find out.

On the PuTTY terminal enter the following to install the latest Laravel version, or specify a particular version number.

{% highlight console %}
username@domain [~]# composer create-project laravel/laravel --prefer-dist
OR
username@domain [~]# composer create-project laravel/laravel=4.2 --prefer-dist
{% endhighlight %}  

If Composer is able to find the particular version you wish to install you should start to see the results of the 
installation. 
{% highlight console %}
Installing laravel/laravel (v4.2.0)
  - Installing laravel/laravel (v4.2.0)
    Downloading: 100%
Created project in /home1/username/laravel
Loading composer repositories with package information
Installing dependencies (including require-dev)
      - Installing symfony/translation (v2.5.11)
        Downloading: 100%
        ...
        ...
Application key [XXX] set successfully.
{% endhighlight %} 

### To remove a Laravel installation simply delete the Laravel folder
If you installed the wrong version, or you need to get rid of Laravel simply delete the folder and its content.

{% highlight console %}
username@domain [~]# rm -r laravel
{% endhighlight %}