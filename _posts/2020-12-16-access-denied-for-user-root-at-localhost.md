---
layout: post
title: "Access denied for user 'root'@'localhost'"
---

One thing that keeps giving me headaches when developing applications, is the underlying configuration of the system you are working on or in. 

A couple of days ago I ran into problems when setting up a new Laravel project on my Ubuntu (20.04) machine. I haven't done so for a couple of month and the first thing that didn't work, was the laravel installer. 
```
laravel installer Server error: `GET http://cabinet.laravel.com/latest.zip` resulted in a `522 Origin Connection Time-out` 
```
Apparently I was on version 3 and that was depracted and the build servers had been shut down ([laravel-news.com/updating-the-laravel-installer](https://laravel-news.com/updating-the-laravel-installer)). That's an easy one: run composer to upgrade to laravel installer version 4 I did:
```
composer global remove laravel/installer 
composer global require laravel/installer
```

Then I figured that I also should upgrade my composer to the newest version 2 ([https://blog.packagist.com/composer-2-0-is-now-available/](blog.packagist.com/composer-2-0-is-now-available)). Alright, that's easy too. But don't forget to sudo the command:
```
 sudo composer self-update --2
```

After successfully upgrading the laravel installer, composer and running the installer the first time to create a new project with laravel 8, I saw that a laravel installation now ships with a docker-compose file ([laravel-news.com/laravel-sail](https://laravel-news.com/laravel-sail)).

'Wow!' I thought. That is awesome. Finally I don't have to worry about system configuration at all. Using laravels docker cli ***sail*** you just say:
```
sail up
```
And **_Boom_!** Error after error I had to fix, to finally be able to run the application and use the mysql database connection. 
First error: 
```
Error starting userland proxy: listen tcp 0.0.0.0:80
```
It took me a while too figure out, that I had too stop the apache and the mysql server on my machine:
```
sudo service apache2 stop
service mysql stop 
```
(I think you can stop these two also using ```systemctl```!?)

Then I was able to start the container but I wasn't able to run laravels migration. I got this error:
```
SQLSTATE[HY000] [2002] Connection refused 
```
It took me a whole day to figure out how to solve it. On my way I also stumbled about an issue with a missing mysql database driver for my local php version. I had to install:
```
sudo apt-get install php7.4-mysql
```
Then installing a laravel project on my machine without a docker container, I ran again into access right conflicts. I solved it by running the following mysql monitor command (server version: 8.0.22-0ubuntu0.20.04.3):
```
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '';
```
The container mysql connection error I finally managed to resolve by setting the value of ```DB_HOST``` in the ```.env``` file to ```mysql```. 
To give you the full picture:
### The docker-compose.yml mysql part
```yml
  mysql:
        image: 'mysql:8.0'
        ports:
            - '${FORWARD_DB_PORT:-3306}:3306'
        environment:
            MYSQL_ROOT_PASSWORD: '${DB_PASSWORD}'
            MYSQL_DATABASE: '${DB_DATABASE}'
            MYSQL_USER: '${DB_USERNAME}'
            MYSQL_PASSWORD: '${DB_PASSWORD}'
            MYSQL_ALLOW_EMPTY_PASSWORD: 'yes'
        volumes:
            - 'sailmysql:/var/lib/mysql'
        networks:
            - sail
```
### The mysql settings in the ```.env``` file
```
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=blog_project
DB_USERNAME=root
DB_PASSWORD=
```
All I wanted was to play around with the laravel application and not bother about these things. OMG why are computers so complicated? 

:sob:

