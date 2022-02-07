---
title: Ubuntu系统下安装LNMP
date: 2021-12-04 11:27:44
tags: [2021,laravel,stub]
categories: [代码]
top_img: /img/ubuntu.jpg
cover: /img/ubuntu.jpg
---

# php

```
# 添加源
sudo apt -y install software-properties-common
sudo add-apt-repository ppa:ondrej/php
# 更新源
sudo apt update
# 安装php8.0-fpm
sudo apt -y install php8.0-fpm
# 安装扩展
sudo apt install -y php8.0-{bcmath,bz2,intl,gd,mbstring,mysql,zip,gd,curl,imagick,xml,redis}
# 检查php配置是否有误
sudo php-fpm8.0 -t
# 重启php
sudo service php8.0-fpm restart
#查看当前安装了哪些版本的PHP
dpkg -l | grep php
```

# nginx

```
sudo nginx=stable 
sudo add-apt-repository ppa:nginx/$nginx
sudo apt update
sudo apt install nginx
```

# mysql

```
wget -c https://repo.mysql.com/mysql-apt-config_0.8.20-1_all.deb
sudo dpkg -i mysql-apt-config_0.8.20-1_all.deb
sudo apt update
sudo apt install mysql-server mysql-client
```

## 添加root用户和密码

```
#查看默认用户和密码
sudo cat /etc/mysql/debian.cnf
```
![](/images/16442037102415.jpg)

```
mysql -u debian-sys-maint -p    # 换行后输入上述查到的密码

use mysql;
# 刷新权限
flush privileges;
# 添加用户
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY '密码';
# 刷新权限
flush privileges;
quit;
# 重启mysql
service mysql restart
```

## 配置外网navicat可访问mysql

```
mysql -u root -p

use mysql；
update user set host='%' where user='root' and host='localhost';
flush privileges;

sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
# 在bin-address  = 127.0.0.1前添加#号
```
![](/images/16442040075215.jpg)

`sudo service mysql restart`
![](/images/16442040753811.jpg)
![WeChatcad30b1452273adfd4b51d8a6e18dae8](/images/WeChatcad30b1452273adfd4b51d8a6e18dae8.png)

