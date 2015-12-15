title: 「基于Swagger-UI+SpringMvc接口文档自动生成」项目
date: 2015-10-14 13:29:45
categories: 项目
tags: [Swagger,SpringMvc]
---

# 简介
`server-api`是一个GUI的Web接口管理工具，基于 [swagger-ui](https://github.com/swagger-api/swagger-ui) 的后台API开源项目，采用SpringMvc+Maven3+Jdk1.7+Tomcat7,该项目的初衷是为了更好的去发展Swagger-ui与SpringMvc的结合，希望能够更加灵活，轻量化的根据后台不同的数据源展示出不同接口文档，满足日益增长的接口文档需求，这里包括（接口文档的格式，权限，视图，Mock数据，Api监控,自动化测试,接口文档分享，版本管理等），`server-api`可以帮助后台开发人员解放双手，直接用注解生成文档，省去与前端、测试的沟通，以及项目上线后问题的监控和排查。


<!-- more -->

# 亮点

- 完美融合Spring3.2.9+,采用注解编写文档，适用于目前大多数项目。
- 最简项目架构，方便快速上手，排除其他干扰，方便学习研究。
- 免费的技术支持，提问请在GitHub上[Issue](https://github.com/mousycoder/server-api/issues)。


# 项目地址
https://github.com/mousycoder/server-api

# 部署

## 准备工作
- Eclipse/MyEclipse/IDEA
- JDK1.7+
- Tomcat7.*+
- Git
- Maven3插件

## 构建项目

### 获取源代码

如果你只是使用

```
git clone git@github.com:mousycoder/server-api.git
git checkout master
```

如果你也想和我一起开发
- 先Fork
- PR一下我的最新代码(develop分支)
- 贡献代码
- PR你的代码到我的仓库

若是好的建议和方案，你将会在作者的仓库看到你贡献的代码，这将是多么让人兴奋的事情。


### 导入IDE
以Eclipse为例，在File->Import->Existing Maven Projects,将server-api项目导入进来。


### 启动项目

- `maven clean install`
- 右键Run As-> Run on server (前提配置好tomcat)

### 日志
- 改变日志级别 src/main/resources->log4j.properties->`log4j.rootLogger=DEBUG, Console`改成ERROR级别 默认是debug调试级别。


# 技术支持


- Issues:[提交您的Issue](https://github.com/mousycoder/server-api/issues)
- QQ群:70812183 
- 文档:[官方Api文档](http://docs.swagger.io/swagger-core/apidocs/index.html)
- 博客:[极简小站](http://mousycoder.com)


---

感谢您的耐心阅读，如果您发现文章中有一些没表述清楚的，或者是不对的地方，请给我留言，你的鼓励是作者写作最大的动力，
如果您认为本文质量不错，读后觉得收获很大，不妨小额赞助我一下，让我更有动力继续写出高质量的文章。

- 支付宝 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/18963137.jpg)
- 微信 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/34122370.jpg)
   
作 者 : [@mousycoder](http://weibo.com/mousycoder)

原文出处 : http://mousycoder.com/2015/10/14/backend-api-project/

创作时间：2015-7-3

更新时间：2015-10-16

