---
layout:     post
title:      "Laravel Valet For Ubuntu"
subtitle:   "在Ubuntu中搭建Laravel Valet开发环境"
date:       2017-01-15 12:00:00
author:     "yang"
header-img: "img/post-bg-04.jpg"
---

# Laravel Valet For Ubuntu 

## install php and extentions

    sudo apt-get install php
    php*-cli php*-common php*-curl php*-json php*-mbstring php*-mcrypt php*-opcache php*-readline php*-xml php*-zip    (like sudo apt install php7.0-xml)

## install mysql and mysql-php
    sudo apt-get install mysql-server mysql-client
    sudo apt install php-mysql

## install composer
    sudo php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
    sudo php composer-setup.php
    sudo mv composer.phar /usr/local/bin/composer
    sudo php -r "unlink('composer-setup.php');"

## use composer install valet
    composer global require cpriego/valet-ubuntu
    if get composer.json is not wirteable use: sudo chown -R $user ~/.composer
    
    use " export PATH=$PATH:~/.composer/vendor/bin"    add ~/.composer/vendor/bin to PATH
    valet install
    
## use valet
    创建一个新目录，例如 mkdir ~/Sites，然后进入这个目录并运行 valet park。这个命令会将当前所在目录作为web根目录。
    接下来，在新建的目录中创建一个新的Laravel站点：     
    composer global require "laravel/installer"
    laravel new blog。
    或者 composer create-project --prefer-dist laravel/laravel  project-name
    
    然后进入项目根目录下运行 valet link project-name
    在浏览器中访问 http://blog.dev。



