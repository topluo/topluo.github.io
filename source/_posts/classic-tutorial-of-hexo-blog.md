title: 「搭建Hexo博客」简明教程
date: 2015-10-19 14:00:31
categories: 教程
tags: [hexo,blog]

photos:
- http://img-storage.qiniudn.com/15-10-19/17724603.jpg
---

[hexo](https://hexo.io/zh-cn/) 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页，相比[jekyll](http://jekyll.bootcss.com/)生成静态网页的速度更快。


<!-- more -->

## 亮点

- 采用最新版本的hexo,最简洁的方式搭建


废话不多说，直接上干货！


## 环境
Win7 旗舰版 64位


## 步骤




### 安装NodeJs


直接进入[官网](https://nodejs.org/en/) Download for Windows(x64) 官网下载比较慢，笔者把所需软件分享在此：http://pan.baidu.com/s/1jGIrbuA   ,安装即可,直接一路下一步即可，网上有用[nvm](https://github.com/creationix/nvm)去安装NodeJs但是个人觉得在这里没必要，小心入坑。


### 安装Git

直接进入[官网](http://git-scm.com/) Download for Windows 目前版本2.6.2 官网也很慢，直接一路下一步即可，


### 安装Hexo


右键选择Git Bash Here,在git命令行工具里输入`$ npm install -g hexo-cli
`,**这里需要翻墙**，分享免费的vpn，ip:207.226.141.213 用户名:free 密码:@Mousycoder@  仅供搭建hexo用。


![](http://img-storage.qiniudn.com/15-10-19/74960459.jpg)

![](http://img-storage.qiniudn.com/15-10-19/91913666.jpg)


#### 本地部署Hexo


- 新建一个存放博客目录的文件夹，例如：blog
- 进入到blog文件夹 `hexo init blog` 然后 `npm install`,博客安装完毕
- `hexo s` 启动博客
- 打开浏览器 http://127.0.0.1:4000/ 即可访问


注意点：

- `npm install` 需要翻墙
- 4000端口被占用会启动不了


#### 部署到Github

举个栗子，用我的github账号

- github上新建一个项目，项目名字叫做mousycoder.github.io,这个是固定的，不能改，一个github用户只能创建一个个人静态网页和一个组织静态网页。


- 配置blog目录下的_config.yml文件，修改deploy参数，其中repo换成刚刚新建项目的git地址，这里用的https，也可以用git形式

```
deploy:
  type: git
  repo: https://github.com/mousycoder/mousycoder.github.io.git 
```

- 在blog目录下，用上面提到的`gitbash`执行`hexo g` `hexo d`命令即可，中途会提示输入用户名和密码
- 在浏览器中输入 http://mousycoder.github.io 即可看到。

想要绑定域名只需

- 把域名cname到 mousycoder.github.io 

- 在source/目录下新建CNAME文件 里面写上 mousycoder.com



### 主题

Hexo 主题大全 https://github.com/hexojs/hexo/wiki/Themes 

个人觉得 [Next主题](https://github.com/iissnan/hexo-theme-next)NexT 是目前最清爽，集成度最好的一个，它坚持将复杂的细节隐藏，提供尽量少并且简便的设置，保持最大限度的易用性，主题的安装和配置上面都已经写的很清楚了，所以这里不再介绍。


### 写文章

举个栗子 写一篇标题名为：你好，分类为：测试，标签为：Hello,链接为：http://127.0.0.1:4000/2015/10/19/hello/  
内容为： 你好  的文章。

- 输入 `hexo new hello`
- 在source/_posts/里找到hello.md 
- 编辑md，内容如下:

```
title: 你好  
categories: 测试  
tags: [Hello]

---
你好

```

- 写完之后，预览没问题，就`hexo g`重新生成下页面,然后`hexo s`在本地浏览器预览。
- 都没问题了，直接`hexo d` 推送到远程github上，发布成功，一般需要等30秒之内，Gitpage有CDN效果。


补充说明：

- 链接可以用时间格式和固定格式，详见：https://hexo.io/docs/permalinks.html
- 可以建立自己的文章模板，避免每次都要输入新的属性，修改scaffolds下的post.md即可，也可以自己新建有一个md，写文章的时候`hexo new [layout] <title>
` 详见：https://hexo.io/docs/writing.html
- 分类页面，标签页面的显示,详见：http://theme-next.iissnan.com/theme-settings.html#分类页面
- 部署到github上一定要重新生成下页面`hexo g`,否则不会更新。
- 用`<!-- more -->` 来分隔预览和阅读全文。




## 使用经验

- 多说评论系统，有时候会不稳定，可以考虑用disqus。
- 语言设定时 language: zh-Hans 才是简体中文，并且中文简体语言配置在 themes/next/languages/zh-Hans.yml。
- 最新版本已经去掉百度分享，详见：https://github.com/iissnan/hexo-theme-next/issues/425。
- [Swiftype](https://swiftype.com/) 是一个非常不错的站内搜索，免费30天，30天后可以转成免费版本，小博客已经够用了，后台可以配置搜索权重，搜索样式，抓取时间，实时分析搜索行为，正则抓取需要被搜索的网页，例如我的是只抓取文章的链接。
- 文章中加入 layout: false 该文本不会被hexo渲染，以纯本文形式存在。
- 写文章上传图片建议用[极简图床](http://yotuku.cn/#!/)配合七牛云。
- windows推荐用markdownpad2，虽然有sublime atom,但是个人觉得markdownpad2实时预览有2个，一个本地应用，一个浏览器预览，非常不错。
- 添加最下面的计数 http://service.ibruce.info/
- Hexo作者博客:http://zespia.tw/blog/2012/10/11/hexo-debut/
- 站长工具推荐
	- 提交搜索引擎：http://www.sousuoyinqingtijiao.com/
	- google站长工具：http://www.google.com/intl/zh-CN/webmasters/
	- 百度站长工具：http://zhanzhang.baidu.com/
	- 百度联盟：http://union.baidu.com/
	- 360网站监控：http://jk.cloud.360.cn/
	- 监控宝：http://www.jiankongbao.com/
	- 域名比价:https://www.domcomp.com/



---

感谢您的耐心阅读，如果您发现文章中有一些没表述清楚的，或者是不对的地方，请给我留言，你的鼓励是作者写作最大的动力，
如果您认为本文质量不错，读后觉得收获很大，不妨小额赞助我一下，让我更有动力继续写出高质量的文章。

- 支付宝 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/18963137.jpg)
- 微信 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/34122370.jpg)
   
作 者 : [@mousycoder](http://weibo.com/mousycoder)

原文出处 : http://mousycoder.com/2015/10/19/classic-tutorial-of-hexo-blog/

创作时间：2015-10-19

更新时间：2015-10-19













