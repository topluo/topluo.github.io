title: 一道面试题引发的思考:(2)
date: 2015-10-14 13:46:24
categories: 一道面试题引发的思考
tags: [java]
---

## 题目

![](http://img-storage.qiniudn.com/15-9-22/50608386.jpg)

答案：D


<!-- more -->

## 分析

### Java 异常的结构体系

![](http://img-storage.qiniudn.com/15-9-22/86147353.jpg)

Throwable 类包含了其线程创建时线程执行堆栈的快照，包含了给出有关错误更多的消息字符串，
有颜色的代表运行时异常，非checked exception，可以不try catch ，则由jvm处理，白色的异常代表checked exceptions必须由try-catch捕获。

### 非RuntimeException

 非runtimeException 一般是外部错误，例如：从文件尾后读取数据，这并不是程序本身的错误，而是应用环境的错误，凡是继承Throwable的，都可以捕获，抛出。

### ERROR

 Error由虚拟机生成并抛出，，属于JVM系统内部错误或者资源耗尽等严重情况，属于JVM需要担负的责任，这一类异常事件是无法恢复或者不可能捕获的，将导致应用程序中断，但是自定义Error是可以捕获的。
 

``` java
package com.mousycoder.error;

public class MyError extends Error{

	public MyError() {
		super();
	}
	
	MyError(String msg) {
		super(msg);
	}
	
	public static void main(String[] args) {
		
		try {
			throw new MyError("error");
		} catch (Throwable e) {
			System.out.println("catching!");
		}
		
	}
	

}
```

### 异常机制

传统异常是由函数返回一个特殊的结果表示，例如： -1表示异常，有时候 -1确是表示正确的值，这样代码可读性差，正确的处理和异常处理代码在一起，异常则由程序员来处理，要求比较高，经过改良之后，程序出现异常，则程序流程发生改变，控制权转到异常处理器，由异常处理器处理，异常处理器也是有很多的，直到找到一个适合的异常处理器，并处理异常。

### 异常的转译

- ERROR到Exception

![](http://img-storage.qiniudn.com/15-9-22/90724665.jpg)

比如讲SQLException转成DAOException,让异常更加准确的表达

``` java
package com.mousycoder.error;

import java.sql.SQLException;

public class DAOException extends Throwable{
	public DAOException() {
		super();
	}
	
	DAOException(String msg,Throwable e){
		super(msg, e);
	}
	
	public static void main(String[] args) throws DAOException {
		SQLException s = new SQLException();
		throw new DAOException("dao异常",s);
	}
	
}
```

**console** 

``` java
Exception in thread "main" com.mousycoder.error.DAOException: dao异常
	at com.mousycoder.error.DAOException.main(DAOException.java:16)
Caused by: java.sql.SQLException
	at com.mousycoder.error.DAOException.main(DAOException.java:15)
```

Spring中DispatcherServlet的doDispatch()方法将Error转成Exception,挽回错误发生带来的负面影响。
```
	private void triggerAfterCompletionWithError(HttpServletRequest request, HttpServletResponse response,
			HandlerExecutionChain mappedHandler, Error error) throws Exception, ServletException {

		ServletException ex = new NestedServletException("Handler processing failed", error);
		if (mappedHandler != null) {
			mappedHandler.triggerAfterCompletion(request, response, ex);
		}
		throw ex;
	}
```


- Exception到RuntimeException

将检查异常转成非检查异常，让代码变得优雅，但是增加了系统发生系统的可能性

- Error到RuntimeException

代码简洁，统一异常处理

### 异常链

将异常的原因一个一个串起来，底层信息传给上层，逐级传递

模型：
```
try {
     lowLevelOp();
    } catch (LowLevelException le) {
     throw (HighLevelException)
      new HighLevelException().initCause(le);
}
```

---


感谢您的耐心阅读，如果您发现文章中有一些没表述清楚的，或者是不对的地方，请给我留言，你的鼓励是作者写作最大的动力，
如果您认为本文质量不错，读后觉得收获很大，不妨小额赞助我一下，让我更有动力继续写出高质量的文章。

- 支付宝 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/18963137.jpg)
- 微信 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/34122370.jpg)
   
作 者 : [@mousycoder](http://weibo.com/mousycoder)

原文出处 : http://mousycoder.com/2015/10/14/thinking-of-interview-question-2/

创作时间：2015-9-22

更新时间：2015-10-14















