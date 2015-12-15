title: 「搭建Ghost博客」经典教程
date: 2015-10-12 20:13:12
categories: 教程
tags:  [ghost,blog]
photos:
- http://img-storage.qiniudn.com/15-10-13/25389879.jpg
- http://img-storage.qiniudn.com/15-10-13/57350801.jpg

---




**[Ghost](http://www.ghostchina.com/)**是一款非常出色的开源博客平台，无论是从架构、设计、易用性，它都要比**[Wordpress](https://cn.wordpress.org/)**要好，界面简洁，专注写作，支持在线预览，在线写作，无论您是在哪里，都可以去写博客，尽情的享受写作带来的快感。

<!-- more -->


## 优势

- 技术上,采用[NodeJs](https://nodejs.org/en)，在可预见的未来里，无疑比[PHP](http://php.net)有更多优势，并发能力远超Wordpress，虽然NodeJs后期维护成本高，但是我们只是借它做博客而已。
- 易用性上，专注写作，评论，超炫皮肤，完美支持 MarkDown,没有Wordpress那么臃肿，回归到博客最原始的状态,传递文字最原始的力量。
- 使用上，便捷，随时随地编辑，比[Hexo](http://hexo.io),[Jekyll](http://jekyllrb.com/)这类静态博客要书写方便，特别是在不同电脑上写作时。


## 劣势

- 需要配套支持Node环境的虚拟机，一般免费的很少支持，这时必须得掏腰包了。
- 后台简陋,许多功能还未完善，不过写作这一块没啥大问题。


## 亮点

- 采用**[Ghost中文版最新版0.6.3](http://www.ghostchina.com/)**保证了最新功能以及教程的实时性,目前已经更新到了0.7.0,教程所写日期时，最新版本为0.6.3。
- 采用Mysql作为数据库,通用快速上手，这里也可以用其他数据库比如Sqlite。
- Nginx作为反向代理,配置多个Ghost博客,同时也能增加了网站的负载。
- 非常简易化的Ubuntu的Node.js安装方法，不用编译打包。
- 安装系统服务,开机重启Ghost服务，免去日后以后操作。
- 采用[Font Awesome](http://fontawesome.io/icons/)作为社交按钮，也可以自定义图标。
- [highlight.js](http://highlightjs.org/) 作为主题的代码高亮引擎
- 整合[Disqus](https://disqus.com/)评论系统,建立属于自己的Discuss圈
- 国外优秀免费[Ghost主题](http://themeforest.net/category/blogging/ghost-themes)资源分享
- 整合[百度统计](http://tongji.baidu.com/web/welcome/login)以及[百度分享](http://share.baidu.com/)
 

**废话不多说，直接上干货！**


## 环境
Ubuntu 14.04,MySql 5.5.43,Nginx 1.4.6,Node 0.10.33
## 步骤

### 安装MySql

``` 
# 安装MySql
$ apt-get update # 更新组件
$ apt-get install mysql-server mysql-client -y # 快速安装-y代表默认选择y省去了回车，这时只需要设置mysql的root密码

# 设置mysql的编码
$ sudo vi /etc/mysql/my.cnf # 搜索到[mysqlld] 插入collation-server = utf8_unicode_ci init-connect = 'SET NAMES utf8' character-set-server = utf8
$ service mysql restart # 重启生效
$ mysql -u root -p # 输入上面设置的密码
$ show variables like 'char%'
$ show variables like 'collation%' # 查看是否改成utf-8了否则之后数据库内存中文存放的是乱码

# 创建Ghost数据库
$ create database mousycoder # 这里把mousycoder换成你想换成的数据库名，建议和域名保持一致，方便以后维护。
$ create database mousycoderDev # 这个是Ghost启动有2种模式 一种开发模式 一种生产模式 这个是开发模式的数据库，如果不想那么麻烦，只用建立一个数据库即可。
$ create user 'mousycoder'@'localhost' identified by '123456' # 这里新建一个用户mousycoder密码为123456,当然我的密码肯定不是123456咯，换成你自己的啦 = =，同样也建议用户名，数据库名，服务名，组名，都和域名保持一致,这里是建立一个只有本地操作的用户，远程无法操作，安全策略。
$ grant all privileges on mousycoder.* to 'mousycoder'@'localhost'
$ grant all privileges on mousycoderDev.* to 'mousycoder'@'localhost' # 这里是赋予mousycoder这个本地用户所有对数据库mousycoder以及mousycoderDev的权限，当然这里你可以根据实际需要赋予权限。
$ FLUSH PRIVILEGES # 重新读取权限表中的数据到内存，不用重启mysql就可以让权限生效，好处可以防止修改错误后，没有余地再去反悔。
```

**补充说明**

- mysql 移除匿名账户，禁用root远程登录： `$ sudo mysql_secure_installation;` 回答n,y,y,y,y
- grant 用法：grant 权限1,权限2,…权限n on 数据库名称.表名称 to 用户名@用户地址 identified by '口令'
其中权限1,权限2,…权限n代表 select,insert,update,delete,create,drop,
index,alter,grant,reload,references,shutdown,process,file14个权限。
例如：`grant select,insert,update,delete,create,drop on mousycoder.employee to 
hello@10.163.225.87 identified by ‘123456′` 
代表给来自10.163.225.87的用户hello分配可对数据库mousycoder的employee表进行select,insert,update,delete,create,drop等操作的权限，并设定口令为123456。
- mysql乱码原因详细解可以参考[曾是土木人](http://www.cnblogs.com/hongfei/archive/2011/12/29/set-names-utf8.html)的博客

### 安装Nginx

``` 
# 安装nginx
$ apt-get install nginx -y 
$ apt-get install curl -y # curl是一种命令行工具，作用是发出网络请求，然后得到和提取数据。
$ curl -i 127.0.0.1 # 确保Nginx 运行，默认监听80端口

# 设置web目录和cache目录
$ mkdir /var/www
$ mkdir -p /var/cache/nginx # -p 可以一下子把中间路径中不存在的文件夹也一起建立，非常实用
$ chown www-data:www-data /var/www # nginx安装会自动建立用户www-data并且默认用这个用户操作
$ chown www-data:www-data /var/cache/nginx

# 修改配置文件(一般不操作这个文件)
$ cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.old # 备份原来配置
$ vi /etc/nginx/nginx.conf # 可以修改默认用户为其他用户

# 为Ghost单独创建nginx配置文件
$ rm /etc/nginx/sites-enabled/default # 删掉默认的配置
$ vi /etc/nginx/sites-available/mousycoder # 建立一个nginx配置文件

```

nginx配置文件

``` 
server {
    listen 0.0.0.0:80; # 监听的端口号
    server_name mousycoder.com; # 把mousycoder.com换成自己的域名，如果没有域名或者网站还没备案下来这里可以写ip,例如120.25.150.209，如果配置多个网站的话，这里可以通过不同的端口对应不同的网站，例如：120.25.150.209:81等 前提是这些端口外网还能访问。
    access_log /var/log/nginx/mousycoder.log;

    location / {
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
        proxy_set_header X-Real-IP $remote_addr;

        proxy_pass http://127.0.0.1:2368;  # 这里是Ghost启动时的默认端口，可以根据实际情况变化，默认也可以
        #proxy_buffering off;
        proxy_redirect off;
    }
}

```

然后重启服务

``` 
$ ln -s /etc/nginx/sites-available/mousycoder /etc/nginx/sites-enabled/mousycoder # 建立软链接到到实际配置路径，方便统一维护配置文件变化。
$ service nginx restart # nginx安装好时已经默认注册了系统的服务，我们就可以直接重启nginx服务，让配置文件生效
```

补充说明

nginx 这里主要是做端口转发映射作用，当然它非常能抗压。

### 安装Node.Js

``` 
$ wget http://nodejs.org/dist/v0.10.39/node-v0.10.39-linux-x64.tar.gz 
$ tar zxf node-v0.10.39-linux-x64.tar.gz && cd node-v0.10.39-linux-x64
$ cp bin/* /usr/bin # 拷贝执行目录，相当于去设置一个环境变量到用户的bin目录
```

补充说明

这里下载的并不是最新版的nodejs，为了**稳定**。
Ghost官网解释
> 从 Ghost 0.6.0 版本开始，Ghost 中文版完整包已经集成了 Nodejs 0.12 版本的 sqlite3 原生库，在 windows（32/64 bit）、Linux（32/64 bit）、Mac（64 bit）操作系统上可以直接在 Nodejs 0.10.x 和 0.12.x 版本上运行。但是，我们强烈建议使用 Node.js 0.10.x 最新版本。对 Node.js 0.12.x 版本的支持还有待考验！ 

详情见 [ghost中文网](http://www.ghostchina.com/ghost-0-6-3-released/)，当然NodeJs有很多种安装方法，个人觉得这种是在这里最适合的方法。

### 安装Ghost

#### 下载Ghost

``` 
$ cd /var/www/
$ curl -L http://dl.ghostchina.com/Ghost-0.6.3-zh.zip -o mousycoder.zip
$ unzip mousycoder.zip -d mousycoder
$ cd mousycoder/
```

#### 配置Ghost

Ghost有两种运行模式：开发模式和产品模式，通过config.js配置
``` 
$ cp config.example.js config.js
$ vi config.js

```
在config.js配置文件里配置

``` 
- production # 生产模式
    production:{
       url: 'http://mousycoder.com',
       main:{},
       database:{
           client :'mysql',
           connection:{
               host:'127.0.0.1',
               user:'mousycoder',  # 数据库连接的用户
               password:'123456',
               database:'mousycoder',
               charset:'utf-8'
           }
       }
    }

- development # 开发模式
    production:{
       url: 'http://mousycoder.com',
       main:{},
       database:{
           client :'mysql',
           connection:{
               host:'127.0.0.1',
               user:'mousycoder',  # 数据库连接的用户
               password:'123456',
               database:'mousycoderDev',
               charset:'utf-8'
           }
       }
    }

```

### 安装与运行

根据package.json 安装依赖包,进入当前mousycoder目录下
``` 
$ cd /var/www/mousycoder
$ npm install --production  # 产品模式；只安装运行的包
$ npm install # 开发模式，默认是开发模式
```


用mousycoder运行Ghost(非root账户运行Ghost更安全)

``` 
$ adduser -shell /bin/bash --gecos 'mousycoder blog' mousycoder
$ chown -R mousycoder:mousycoder /var/www/mousycoder
```
安装forever,保持Ghost一直在后台运行

``` 
$ cd /var/www/mousycoder
$ npm install forever -g # 全局安装forever模块
$ NODE_ENV=production forever start index.js # 生产模式后台运行ghost
```

安装系统服务
``` 
$ curl https://raw.githubusercontent.com/TryGhost/Ghost-Config/master/init.d/ghost -o /etc/init.d/mousycoder # 下载Ghost提供的脚本到/etc/init.d/目录，该目录是系统服务目录
$ chmod +x /etc/init.d/mousycoder # 给脚本赋予执行权限
$ usermod -aG mousycoder www-data # 把www-data用户加入mousycoder组，让其可以操作源文件等目录
$ update-rc.d mousycoder defaults # 用update-rc.d 安装服务 mousycoder
$ update-rc.d mousycoder enable # 刷新一遍服务,防止之前有重名的
$ service mousycoder status # 查看mousycoder 服务的状态
$ service mousycoder start # 这样开机就会自动启动ghost生产环境，不信reboot一下
```
补充说明

- curl 常用命令 详情可以参考[阮一峰](http://www.ruanyifeng.com/blog/2011/09/curl.html)的博客

``` 
$ curl -L https://raw.githubusercontent.com/TryGhost/Ghost-Config/master/init.d/ghost -o ghost # -L 解决网站地址自动跳转后拿不到文件
$ curl -v www.baidu.com  # 显示详细过程包含http头
$ curl -u username:password url 解决页面需要授权输入用户名密码情况
$ curl -u username --data "param1=value1&param2=value2" https://api.github.com  # post请求
$ curl -I -X DELETE https://api.github.com  # 解决get post以外的请求方式
$ curl --form "fileupload=@filename.txt" http://hostname/resource  # 上传文件
```

- chmod 命令

``` 
$ chmod -R a-w abc # 取消/abc目录的-w（写）权限
```
drwxr-xr-x 第一列d 目录 第2-4列 拥有者权限 rwx 5-7列 r-x同组用户权限 r-x是其他组用户权限,其中rwx对应4,2,1 

- 系统服务启动顺序

``` 
$ update-rc.d A start 50 1 2 3 4 5 stop 51 0 6
$ start 50 1 2 3 4 5 # 表示在1,2,3,4,5这5个运行级别中，按先后顺序，由小到大执行，第50个开始运行脚本
$ stop 50 0 6 # 表示在0，6这两个运行级别中，按照先后顺序，由小到到执行，第52个停止这个脚本运行
$ update-rc.d mousycoder remove # 卸载mousycoder开机服务
```

### 启动
打开浏览器，输入之前配置的ip或者域名


首页 http://hostname.com/ghost 
Ghost后台 http://hostname.com/ghost   


现在你就可以尽情享受Ghost带给你的极致简洁了，快来书写吧！



## 主题
 

![](http://ww4.sinaimg.cn/large/74311666jw1ex0g0kbnfhj20y20lkah6.jpg)



![](http://ww4.sinaimg.cn/large/74311666jw1ex0fxvvaubj210q0ldn61.jpg)


![](http://ww3.sinaimg.cn/large/74311666jw1ex0fyx4780j212q0ob7aw.jpg)

主题地址：https://github.com/mousycoder/mouse

欢迎fork
 

## 附录

- [HighLight使用教程](https://highlightjs.org/usage/)



---




感谢您的耐心阅读，如果您发现文章中有一些没表述清楚的，或者是不对的地方，请给我留言，你的鼓励是作者写作最大的动力，
如果您认为本文质量不错，读后觉得收获很大，不妨小额赞助我一下，让我更有动力继续写出高质量的文章。

- 支付宝 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/18963137.jpg)
- 微信 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/34122370.jpg)
   
作 者 : [@mousycoder](http://weibo.com/mousycoder)

原文出处 : http://mousycoder.com/2015/10/12/classic-tutorial-of-ghost-blog/

创作时间：2015-7-1

更新时间：2015-10-14




