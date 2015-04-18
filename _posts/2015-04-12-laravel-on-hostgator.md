---
layout: post
title: Installing Laravel on Hostgator
---

Have you developed a site using Laravel, and are now getting ready to deploy it? One of the first things to 
consider is where to host your site, and make sure that hosting provider supports the minimum [server requirements]
(http://laravel.com/docs/4.2#server-requirements) for Laravel.

The instructions below are specific to installing Laravel on a Hostgator shared instance. However, this shouldn't be 
too different even if you're using a different hosting provider.

### Sign up for a Web Hosting Service
If you don't already have a Hosting account, now is the time to get one! Here's a link to [HostGator]
(http://secure.hostgator.com/~affiliat/cgi-bin/affiliates/clickthru.cgi?id=) along with a discount coupon code 
**QuickDeploy** to get you started!

### Connect to your hosting account terminal
Once you have a hosting account, you'll need to access its terminal. You can use any SSH client to do so. In my 
case I use [PuTTY](http://www.putty.org/). You'll also need the server IP Address ready. If you don't have this 
already, browse over to your Hosting Service Account or CPanel to find it.

PuTTY Configuration  

*    Session:  
     > Host Name (or IP Address): *Your domain name, or IP*   
     > Port: *Default is 22, but if you use a shared instance try 2222*    
     > Connection type: *SSH*  
*    Connection: Data  
     > Auto-login username: *Your HostGator username*  

I used default settings for everything else. If the above doesn't work and you need more help check out [this 
link](https://support.hostgator.com/articles/specialized-help/technical/ssh-keying-through-putty-on-windows-or-linux)

### Check PHP version

Once connected, check the PHP version of your hosted instance. You'll need to have at least [PHP 5.4](http://laravel
.com/docs/4.2#server-requirements) to install Laravel. 

{% highlight console %}
username@domain [~]# php -v
PHP 5.4.38 (cli) (built: Mar 11 2015 12:43:38)
{% endhighlight %}  

### Install Composer
While you don't have to do this, i.e. you could just copy your local Laravel folder, or clone using Git, 
having Composer makes it much easier to install Laravel and its related packages.

{% highlight console %}requested
username@domain [~]# curl -sS https://getcomposer.org/installer | php
All settings correct for using Composer
Downloading...
Composer successfully installed to: /home1/pernalin/composer.phar
Use it: php composer.phar
{% endhighlight %}  

### Install Laravel
Install the right version of Laravel on your Host. If you don't know which Laravel version you have in Dev, use `php 
artisan --version` on your local dev console to find out.

On the PuTTY terminal enter the following command to install. Make sure you replace 'version number' with the 
required Laravel version. E.g. `laravel/laravel=**4.2** --prefer-dist`

{% highlight console %}
username@domain [~]# php composer.phar create-project laravel/laravel=<version number> --prefer-dist
{% endhighlight %}  

If you don't specify a version number, composer will install the latest distribution.
{% highlight console %}
username@domain [~]# php composer.phar create-project laravel/laravel --prefer-dist
{% endhighlight %}  

If Composer is able to find the requested version, it will begin the install.
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

### To remove a Laravel installation
If you installed the wrong version, or you need to get rid of Laravel just delete the Laravel folder and its 
content from your hosting site.

{% highlight console %}
username@domain [~]# rm -r laravel
{% endhighlight %}