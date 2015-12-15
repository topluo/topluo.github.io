title: 在ubuntu14.04上安装lamp环境
date: 2015-11-25 13:25:04
categories: 运维
tags: [lamp,ubuntu]
---

## 配置安装源

编辑sources.list文件，替换一下更新源，这里用阿里云的更新源
`vim /etc/apt/sources.list`

阿里云更新源


<!-- more -->

```

deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse

deb http://mirrors.aliyuncs.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyuncs.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyuncs.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyuncs.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyuncs.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.aliyuncs.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.aliyuncs.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.aliyuncs.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.aliyuncs.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.aliyuncs.com/ubuntu/ trusty-backports main restricted universe multiverse


```


中科大更新源

```
deb http://debian.ustc.edu.cn/ubuntu/ trusty main restricted universe multiverse
deb http://debian.ustc.edu.cn/ubuntu/ trusty-security main restricted universe multiverse
deb http://debian.ustc.edu.cn/ubuntu/ trusty-updates main restricted universe multiverse
deb http://debian.ustc.edu.cn/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://debian.ustc.edu.cn/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://debian.ustc.edu.cn/ubuntu/ trusty main restricted universe multiverse
deb-src http://debian.ustc.edu.cn/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://debian.ustc.edu.cn/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://debian.ustc.edu.cn/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://debian.ustc.edu.cn/ubuntu/ trusty-backports main restricted universe multiverse
```

更新

`apt-get update`


## 安装Mysql

`apt-get install mysql-server mysql-client`

查看Myql默认密码

`vim /etc/mysql/debian.cnf`

用默认密码进入mysql,修改Mysql默认密码

`set password for 'root'@'localhost' = password('yourpass')`

允许myysql外部链接

修改 /etc/mysql/my.cnf，注释掉bind-address = 127.0.0.0


## 安装Apache

`apt-get install apache2`

默认网站目录 /var/www/html 

修改默认网站目录 

`/etc/apache2/sites-available/000-default.conf` 把DocumentRoot换成自己想指定的目录

其中如果要绑定域名的话 修改`ServerName XX.COM`



## 安装PHP

`-get install php5 libapache2-mod-php5`

重启apache2 

`service apache2 restart`

## 测试 PHP，建立一个探针文件

`vi /var/www/html/info.php`

内容 
```
<?php
phpinfo()
?>
```

打开 localhost/info.php 可以看到

![](http://static.mousycoder.com/15-11-25/28406513.jpg)



## 让PHP5支持MySql

`apt-cache search php5
apt-get install php5-mysql php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl`

重启apache2 

`service apache2 restart`


## 安装 XCache 优化缓存

`apt-get install php5-xcache`


重启apache2 

`service apache2 restart`

## 安装phpmyadmin管理Mysql

`apt-get install phpmyadmin`

![](http://static.mousycoder.com/15-11-25/13970942.jpg)
![](http://static.mousycoder.com/15-11-25/81627900.jpg)

要求输入Myql的用户名和密码。


如果此步跳过，以后想重新配置

`dpkg-reconfigure phpmyadmin`

phpmyadmin在/usr/share/phpmyadmin目录，所以就用命令：`ln -s /usr/share/phpmyadmin /var/www` 建立软连接。

phpadmin地址：localhost/phpmyyyadmin/index.php

---



感谢您的耐心阅读，如果您发现文章中有一些没表述清楚的，或者是不对的地方，请给我留言，你的鼓励是作者写作最大的动力，
   
作 者 : [@mousycoder](http://weibo.com/mousycoder)

原文出处 : http://mousycoder.com/2015/11/25/install-lamp-environment-on-ubuntu14-04/


创作时间：2015-11-25