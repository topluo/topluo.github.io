title: 一道面试题引发的思考:(1)
date: 2015-10-12 15:55:35
categories: 一道面试题引发的思考
tags:  [java]
---

这是网易2015校招Java面试题,直接上题目。

## 题目

```
package com.mousycoder.staticTest;

public class HelloB extends HelloA {
	public HelloB() {
		System.out.println("HelloB");
	}

	{
		System.out.println("I’m B class");
	}
	static {
		
		System.out.println("static B");
	}

	public static void main(String[] args) {
		new HelloB();
	}
}

class HelloA {
	public HelloA() {
		System.out.println("HelloA");
	}

	{
		System.out.println("I’m A class");
	}
	static {
		System.out.println("static A");
	}
}
```



<!-- more -->

## 答案
```
static A
static B
I’m A class
HelloA
I’m B class
HelloB()

```

## 考察点

- static 相关知识
- 非静态代码块相关知识
- 类加载顺序


## 解析

- 存在父子关系，又有静态代码块，先执行父类静态代码块，再执行子类静态代码块，故打印`static A` `static B`

- 存在父子关系，又有非静态代码块，先执行父类非静态代码块，父类构造器，再执行子类非静态代码块，子类构造器故打印`I'm A class` `HelloA` `I'm B class` `HelloB`


## 结论

- 静态代码块=>非静态代码块=>构造方法
- 父子关系：父类静态代码块=>子类静态代码块=>父类非静态代码块=>父类构造函数=>子类非静态代码块=>子类构造函数


## 扩展知识

### 非静态代码块

```
{ 
   System.out.println("I'm B class");
}
```

这个叫做非静态代码块，也叫普通代码块，在每个类创建前(构造函数之前)调用 ，不创建对象的时候，不被调用，代码块中定义的变量都是局部变量，如果有父子关系的话，先执行父类的，再执行子类的。非静态代码块属于对象，静态代码块属于类。


```
package com.mousycoder.staticTest;

public class Child extends Father{
	
	static {
		System.out.println("child-->static");
	}
	
	private int n = 20;
	
	{
		System.out.println("Child Non-Static");
		n = 30;
	}
	
	public int x = 200;
	
	public Child() {
		this("The other constructor");
		System.out.println("child constructor body: " + n);
	}
	
	public Child(String s) {
		System.out.println(s);
	}
	
	public void age() {
		System.out.println("age=" + n);
	}
	
	public void printX() {
		System.out.println("x=" + x);
	}
	
	public static void main(String[] args) {
		new Child().printX();
	}
}

class Father {
	
	static {
		//System.out.println("n+"+n);
			//当n定义在下面时，会提示Cannot reference a field before it is defined，
			//所以必须把n定义移到上面才可以输出
		System.out.println("super-->static");
	}
	
	public static int n = 10;
	public int x = 100;
	
	public Father() {
		System.out.println("super's x=" + x);
		age();
	}
	
	{
		System.out.println("Father Non-Static");
	}
	
	public void age(){
		System.out.println("nothing");
	}
}
```

结果:
```
super-->static
child-->static
Father Non-Static
super's x=100
age=0
Child Non-Static
The other constructor
child constructor body: 30
x=200
```

解析：

- 先执行静态代码块，有父子关系，先执行父类静态代码块，打印`super-->static`
- 执行子类静态代码块，打印`child-->static`
- 实例化子类的时候，先实例化父类，在调用父类构造方法之前，先调用父类非静态代码块，打印`Father Non-Static`
- 实例化父类，调用父类构造方法，执行第一句 System.out.println("super's x=" + x); 在调用构造方法之前，成员变量x 已经被赋值为100，打印`super's x=100`
- 执行age()方法，因为main方法中真正实例化的是子类，**子类又重写了父类的age方法，所以执行子类的age方法，此时子类还没初始化，默认n=0 ,打印`age=0`**
- 实例化子类，调用子类构方法之前，调用子类非静态代码块，打印`Child Non-Static`
- 调用子类构造方法，执行this("The other constructor"),调用子类的Child（String s），打印`The other constructor`
- 执行System.out.println("child constructor body: " + n); 初始化之前 成员变量初始值为20，执行完非静态代码块后 n被修改成30 ，所以这里n =30 打印`child constructor body: 30`
- 执行printX()方法，打印`x = 200`


注意点：

- 子类重写父类的方法
- 虽然非静态代码块里是局部变量，但是可以改变类的成员变量的值

### 静态代码块

```
static {
  System.out.println("static B");
}

```
属于静态代码块，使用场景：1.想用一个存储区域来保存一个特定的数据，不想创建对象 2.创建一个特殊的方法，与这个类的对象没有关联，即使没创建对象，也可以调用


### 静态变量

静态变量也叫做static变量，静态变量和非静态变量的区别在于静态变量被所有的对象所共享，**在内存中只有一个副本**，它当且仅当类初次加载时会被初始化，而非静态变量是对象所拥有的，在创建对象的时候被初始化，**存在多个副本，各个对象拥有的副本互不影响** 好处：1.可以用类名直接访问(方便),也可以用对象访问(不推荐) 2.只分配一次内存，节省空间


### 静态方法

静态方法就是不会被this和super的方法。特点：1.在static方法内部不能调用所属类的实例变量和实例方法（非静态方法），但是可以在没有创建任何对象的前提下，仅仅通过类本身(类名)来调用static方法 2.main方法就是static 方法，因为执行main方法的时候，没有创建任何对象，只能通过类名去访问 3.没有显示声明，类的构造器也是静态方法,参考：[实例构造器是不是静态方法？](http://rednaxelafx.iteye.com/blog/652719)
常见的错误`Cannot make a static reference to the non-static field` 就是静态方法中引用了非静态的的变量，静态变量是不依赖对象而存在的，非静态变量是依赖对象而存在的


示例：非静态方法可以访问静态变量、方法，**静态方法是不可以访问非静态变量，方法**
```
package com.mousycoder.staticTest;

public class Test {
	
	private static String str1 = "111";
	
	private String str2 = "ddd";
	
	
	public Test() {
	}
	
	public  void print1() {
		System.out.println(str1);
		System.out.println(str2);
		print1();
	}
	
	
	public static void print2(){
		System.out.println(str1);
		System.out.println(str2);  //报错  Cannot make a static reference to the non-static field
		print1(); //报错
	}

}
```

### 静态代码块

由static 关键字修饰的代码块叫做静态代码块，特点：用于提高程序性能，类在初始化的时候，JVM会按照他们在类中出现的顺序加载static 代码块，并且只会执行一遍

示例：只生成一个String s 对象和一个String m 对象
```
package com.mousycoder.staticTest;

import java.util.Date;
public class HelloStatic {
	
	public static String s ;
	
	public static String m ;
	
	static {
		s = new String("hell");
		m = new String("0");
	}
	    String hello() {
	        String s = "hell";
	        String m = "o";
	        return s + m;
	    }
	    
	    public static void main(String[] args) {
	    	Date start = new Date();
	    	for (int i = 0; i < 1000000000 ; i++) {
	    		HelloStatic h = new HelloStatic();
		    	h.hello();
			}
	    	Date end = new Date();
	    	System.out.println(end.getTime() - start.getTime());
		}

}
```

耗时：12ms 

示例： 生成N个String s和String m对象
```
package com.mousycoder.staticTest;

import java.util.Date;

public class Hello {
	    
	    String hello() {
	        String s = new String("hell");
	        String m = new String("o");
	        return s + m;
	    }
	    
	    public static void main(String[] args) {
	    	Date start = new Date();
	    	for (int i = 0; i < 1000000000; i++) {
	    		Hello h = new Hello();
		    	h.hello();
			}
	    
	    	Date end = new Date();
	    	System.out.println(end.getTime() - start.getTime());
		}
	    
}

```
所用时间 9645ms

公共的变量应该放到static里初始化，有利于提高效率

### 静态变量访问权

static 变量是被所有对象共享的，只要该对象有权限，就可以访问该变量

```
package com.mousycoder.staticTest;

public class Main {

    static int value = 33;
 
    public static void main(String[] args) throws Exception{
        new Main().printValue();
    }
 
    private void printValue(){
        int value = 3;
        System.out.println(this.value);
    }
}
```

该输出为：33 ,new Main()创建了一个对象，通过this.value是访问的该对象的value 而int value = 3 是局部变量，并不是和该对象相关联的，因为static变量是共享的，所以该对象可以访问到value 值为 33 ，把this 去掉 则结果为 3


### 静态变量相关面试题

```
package com.mousycoder.staticTest;

public class Test {
    Person person = new Person("Test");
    static{
        System.out.println("test static");
    }
     
    public Test() {
        System.out.println("test constructor");
    }
     
    public static void main(String[] args) {
        new MyClass();
    }
}
 
class Person{
    static{
        System.out.println("person static");
    }
    public Person(String str) {
        System.out.println("person "+str);
    }
}
 
 
class MyClass extends Test {
    Person person = new Person("MyClass");
    static{
        System.out.println("myclass static");
    }
     
    public MyClass() {
        System.out.println("myclass constructor");
    }
}
```

顺序是： 
```
test static
myclass static
person static
person Test
test constructor
person MyClass
myclass constructor
```
执行顺序

```
test static =>new MyClass() => myclass static => new MyClass() => person static => Person person = new Person("Test") => person Test => Person person = new Person("Test") => test constructro => Person person = new Person("Myclass") => person MyClass => Person person = new Person("MyClass") => myclass contructor
````

说明：

- 执行main方法，实例化MyClass 
- 发现MyClass 有父类Test，首先实例化Test
- 加载Test类，这个时候会执行static方法，打印 `test static `, 
- 然后要实例化MyClass 类，加载MyClass类，执行static方法，打印`myclass static`
- 在所有类按照顺序都加载完毕后，开始一个一个实例化类，先实例化父对象 Test类
- 在实例化之前，要先**初始化成员变量**，这个时候会执行`Person p = new Person("Test")`,这个时候发现Person类还没有被加载过，因此先加载Person类
- 执行Person类的static方法，打印`person static `接着实例化Person p = new Person("Test") 调用Person的构造方法，打印`person Test`
- 然后开始调用Test的构造方法,打印`test constructor`,Test类初始化完毕
- 开始实例化Myclass类，首先还是初始化MyClass的成员变量 ,**因为Person已经加载过了,所以只执行一遍**,只需根据类的模板执行其构造方法，来复制成另外一个副本,此时打印`person MyClass`
- 最后执行Myclass的构造方法，打印`myclass constructor`


总结：

非静态代码块=>初始化成员变量=>非静态代码块=>构造器方法


---


感谢您的耐心阅读，如果您发现文章中有一些没表述清楚的，或者是不对的地方，请给我留言，你的鼓励是作者写作最大的动力，
如果您认为本文质量不错，读后觉得收获很大，不妨小额赞助我一下，让我更有动力继续写出高质量的文章。

- 支付宝 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/18963137.jpg)
- 微信 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/34122370.jpg)
   
作 者 : [@mousycoder](http://weibo.com/mousycoder)

原文出处 : http://mousycoder.com/2015/10/12/thinking-of-interview-question-1/

创作时间：2015-7-1

更新时间：2015-10-14


