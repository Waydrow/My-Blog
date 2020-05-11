---
title: LAMP环境配置初体验
date: 2016-02-19 20:59:37
toc: true
categories: coding
tags:
- linux
- ubuntu
- 服务器
---

其实这算是一篇迟到的文章，前段时间用`ubuntu`时记录下来的，今天抽空整理下来。并没有什么干货，只是记录在此备用。
众所周知，`LAMP` 指的就是`Linux`,`Apache`,`MySQL`,`PHP`，在windows上有大杀器————`wampserver`，但是在Linux上就需要自己一步步配置了。
<!-- more -->
##  安装Apache
``` bash
sudo apt-get install apache2
```
安装完毕后在浏览器中输入：`localhost`，或`127.0.0.1` (就是本机地址咯)
显示如下图，则安装成功：
![](https://raw.githubusercontent.com/Waydrow/PicGo/master/img/imagesImage.png)

## 安装PHP
### 安装
``` bash
sudo apt-get install php5 libapache2-mod-php5
```

### 重启apache
``` bash
sudo service apache2 restart
```

### 测试PHP
``` bash
sudo gedit /var/www/html/info.php
```
内容为：

```
<?php
phpinfo();
?>
```

访问 `localhost/info.php`，若出现一堆php相关信息则安装成功

## 安装MySQL
``` bash
sudo apt-get install mysql-server mysql-client
```

系统提示设置密码：

![](https://raw.githubusercontent.com/Waydrow/PicGo/master/img/imagesImage1.png)

![](https://raw.githubusercontent.com/Waydrow/PicGo/master/img/imagesImage2.png)

`New password for the MySQL “root” user: `<– 输入你的root密码
`Repeat password for the MySQL “root” user: `<– 再输入一次


紧接着改写`/var/www`目录的权限。方便日后编辑网站文件。
输入：

``` bash
sudo chmod 777 /var/www
```

## 让PHP支持MySQL

``` bash
sudo apt-cache search php5

sudo apt-get install php5-mysql php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl
```

### 重启 apache
``` bash
sudo service apache2 restart
```

## 安装 XCache 优化缓存
``` bash
sudo apt-get install php5-xcache
```

### 重启 apache:
``` bash
sudo service apache2 restart
```

## 安装phpmyadmin管理Mysql
### 安装
``` bash
sudo apt-get install phpmyadmin
```

![](https://raw.githubusercontent.com/Waydrow/PicGo/master/img/imagesImage3.png)

选择第一个

### 设置phpmyadmin
![](https://raw.githubusercontent.com/Waydrow/PicGo/master/img/imagesImage4.png)

![](https://raw.githubusercontent.com/Waydrow/PicGo/master/img/imagesImage5.png)

### 创建phpmyadmin链接
```bash
sudo ln -s /usr/share/phpmyadmin /var/www/html/
```
现在在浏览器中输入：`localhost/phpadmin`
登陆后就能正确显示管理界面。
![](https://raw.githubusercontent.com/Waydrow/PicGo/master/img/imagesImage6.png)

## 后记
如有问题，欢迎直接在下方讨论留言
我的邮箱为：<Waydrow@163.com>