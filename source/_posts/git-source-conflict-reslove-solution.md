title: 「Git SourceTree冲突」解决方案
date: 2015-10-14 13:28:32
categories: 方案
tags: [Git,SourceTree]
---


现在程序猿标配GIT作为代码管理，但是从SVN到GIT学习中，其中GIT的冲突是一个难点，常常会导致Push不上去,Pull不下来，很尴尬的地步，还不知道自己写的代码被覆盖没，废话不多说，直接上干货！

---

<!-- more -->

## 亮点

采用`SourceTree插件`和`BeyondCompare` 可视化解决冲突


## 方法

### 构造冲突

- A 修改了conflict.file 中第1行内容并且提交到git上
- B 这个时候也修改了confilct.file中第一行内容准备提交，这个时候git就会提示

```
To git@192.168.x.xxx:xxx/server-aggregator.git
 ! [rejected]        develop -> develop (fetch first)
error: failed to push some refs to 'git@192.168.xx.xx:xxx/server-aggregator.git'

hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
提示远程已经有更新了，本地版本太低，让我们先pull拉取最新的代码。

- 我们pull一下，这个时候由于本地有修改这个文件，就会在本地产生冲突文件

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-7-1/74789568.jpg)

### 配置外部比较工具

- 下载[Beyond Compare](http://www.scootersoftware.com/download.php)
- 打开SourceTree->工具->选项->比较->外部差异对比合并->选择BeyondCompare

### 解决冲突

- 在本地副本->右键->解决冲突->打开外部合并工具
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-7-1/63473198.jpg)

- 和svn一样解决好冲突保存更改，退出即可
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-7-1/92149529.jpg)
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-7-1/2384601.jpg)

### 另外一种情况

- 拉取时出现如下提示：
```
it -c diff.mnemonicprefix=false -c core.quotepath=false pull local-server-aggregator develop
/opt/gitlab/embedded/service/gitlab-shell/bin/gitlab-shell:3: warning: Insecure world writable dir /usr in PATH, mode 040777
From 192.168.0.200:weitoo/server-aggregator
 * branch            develop    -> FETCH_HEAD
Updating b0c5c94..40cef3b
error: Your local changes to the following files would be overwritten by merge:
	server/conflict.file
Please, commit your changes or stash them before you can merge.
Aborting
```

**提示需要暂存本地修改，才能拉取服务器上新的代码**

- 点击贮存(英文版:Stash),随便起一个名字，里面存的都是距离上次服务器版本到本地修改之间的差异，千万别删掉了,合并成功无误了再删掉。

- pull拉取服务器代码，这个时候，本地的代码变成了服务器上的代码。
- 点击贮藏->应用贮藏区 ，这个时候是把之前的修改合并到本地上，这个时候会提示冲突。
```
git -c diff.mnemonicprefix=false -c core.quotepath=false stash apply stash@{0}
Auto-merging server/conflict.file
CONFLICT (content): Merge conflict in server/conflict.file
```
可以在sourcetree里看到有感叹号，代表冲突文件，和上面解决冲突方法类似，但是稍微不同，最左边成了远程版本，中间为远程上一个版本，最后才是本地修改。
这个是和我们操作方式有关：**我们是先暂存本地修改，先拉取远程代码，这个时候local 就成了远程代码，最后我们用暂存的合并进去，remote就成了本地修改**

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-7-1/63052008.jpg)

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-7-1/51143764.jpg)

### 多余的.orig文件

这个是由于git自身造成的 它会解决冲突后 生成一个原来冲突的备份，我们可以去掉

`git config --global mergetool.keepBackup false`


---

感谢您的耐心阅读，如果您发现文章中有一些没表述清楚的，或者是不对的地方，请给我留言，你的鼓励是作者写作最大的动力，
如果您认为本文质量不错，读后觉得收获很大，不妨小额赞助我一下，让我更有动力继续写出高质量的文章。

- 支付宝 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/18963137.jpg)
- 微信 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/34122370.jpg)
   
作 者 : [@mousycoder](http://weibo.com/mousycoder)

原文出处 : http://mousycoder.com/2015/10/14/git-source-conflict-reslove-solution/

创作时间：2015-6-15

更新时间：2015-10-16


