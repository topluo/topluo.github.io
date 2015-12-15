title: 我更喜欢用 Intellij IDEA 部署应用
date: 2015-10-14 13:33:28
categories: 翻译
tags: [idea]
---

不管你相不相信，但是我已经用 [Intellij IDEA](http://www.jetbrains.com/idea/) 超过10年了，并且我非常喜欢它。因为如此，我去的每一个会议，我都会去拜访一下[JetBrains](http://www.jetbrains.com/)的摊位，并且和这群小伙子聊天。主要是告诉他们的产品有多好，同时也给他们一些可能的改进想法(我已经告诉他们在MAC OSX用法语键盘使用IntelliJ IDEA是一件没涵养的事情)。所以上次我在[Devoxx UK](http://www.devoxx.co.uk/)(一个关注Java,web,mobile和JVM语言的会议)上和 [Hadi Hariri](https://twitter.com/hhariri) 谈论了在IDE中如何更好的支持应用服务器(这里说的是[WildFly](http://www.wildfly.org)),他让我给他发一封邮件...所以我写了这篇文章。

<!-- more -->

## Intellij IDEA对应用服务器的支持

目前Intellij IDEA在以下方面完美支持大部分[应用服务器](http://www.jetbrains.com/idea/features/java_ee.html):(常见的有：Glassfish, JBoss, WebLogic, WebSphere, Tomcat, Jetty, Geronimo, Resin)

- 服务器管理(开始和停止本地或者远程服务器实例)
- 用断点一步一步执行，判断表达式等去调试Java和JSP文件
- 自动部署或者卸载Web,JavaEE和EJB模块

就GUI而言，多窗口分离，方便你可视化服务器日志文件，开始、停止、重新部署。Run/Debug视图(下面)允许你开始、停止应用服务，部署、卸载、刷新你的组件。

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-8-4/49822893.jpg)

应用服务器窗口有一点小小的失望因为他没有提供更多的信息：

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-8-4/82832779.jpg)

它还有许多我们想看到的特性，但是我们现在先对比一下[Eclipse](https://www.eclipse.org/) 和 [NetBeans](https://netbeans.org/)对应用服务器的支持吧~

## Eclipse对应用服务器的支持
Eclipse服务器窗口比Intellij IDEA提供更多的功能，当然它有运行、调试、部署、卸载特性，但是它提供一些更加友好的额外信息。

首先，我能看到我的组件内容(比如一个EAR文件)，我能右击它，并且在文件浏览器中找到他(非常方便的让你hack一些文件)，同时你也能很快的看到当前服务器监听的端口(这里是9999和8080)，最棒的是，你可以直接访问配置文件(这里是`standalone.xml`),右键，编辑，改变文件内容，保存内容，重新部署，非常的方便。

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-8-4/26425151.jpg)

## NetBeans对应用服务器的支持

NetBeans对 GlassFish 完美支持。和其他的IDE一样，你可以开始、停止你的应用服务器，部署、卸载你的组件。与此同时，它会给你一些部署资源的信息。

在这个服务器窗口你可以看到部署好的JDBC数据源，连接池，JMS连接工厂以及目的地。不过这些信息只是可读。

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-8-4/21813751.jpg)


你想看到Intellij IDEA哪些新特性呢？

在我看来， Intellij IDEA 能和其他IDE一样友好的支持app应用服务器，甚至比他们更好。这也是我为什么一直用 Intellij IDEA 的原因。

- 不用部署组件就可以启动一个app应用服务(这样我可以快速的启动服务，并且检查admin控制台)
- 像Eclipse一样，可视化部署的组件内容以及在文件浏览器中找到他们
- 可视化端口信息，并且改变他们（这样我可以更容易的用不同端口启动多个服务）
- 直接访问配置文件(像`standalone.xml`)，并且可以去改变、保存它的内容
- 可视化部署的资源(数据源、消息目的地)

你呢，想让Intellij Idea 拥有什么特性呢？

## 参考

- [IntelliJ IDEA 13, Java EE 7 and WildFly (Tech Tip #4)](http://blog.arungupta.me/intellij-idea-13-javaee7-wildfly-tech-tip-4/)
- [JetBrains Ships IntelliJ IDEA 13 With Enhanced Java EE 7 Support](http://www.eweek.com/enterprise-apps/jetbrains-ships-intellij-idea-13-with-enhanced-java-ee-7-support.html)


---




感谢您的耐心阅读，如果您发现文章中有一些没表述清楚的，或者是不对的地方，请给我留言，你的鼓励是作者写作最大的动力，
如果您认为本文质量不错，读后觉得收获很大，不妨小额赞助我一下，让我更有动力继续写出高质量的文章。

- 支付宝 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/18963137.jpg)
- 微信 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/34122370.jpg)
   
作 者 : [@mousycoder](http://weibo.com/mousycoder)

译文出处 : http://mousycoder.com/2015/10/14/i-would-like-better-appserver-support-in-intellij-idea

原文出处 : http://antoniogoncalves.org/2014/08/22/i-would-like-better-appserver-support-in-intellij-idea/

创作时间：2015-8-1

更新时间：2015-10-14



