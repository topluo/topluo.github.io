title: 【系统架构师修炼之道】（13）：操作系统基础知识——进程基础知识
date: 2015-10-14 13:41:34
categories: 系统架构师修炼之道
tags:
---

## 概念

是计算机中的程序关于某数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位。



<!-- more -->
## 进程分类(性质)

1. 系统进程 
windows常见的有 
 - dllhost.exe(DCOM DLL Host进程支持基于COM对象支持DLL以运行Windows程序)
 - kernel32.dll(Windows壳进程用于管理多线程、内存和资源) 
 - mprexe.exe(Windows路由进程包括向适当的网络部分发出网络请求)
 - snmp.exe(Windows简单的网络协议代理（SNMP）用于监听和发送请求到适当的网络部分)
 - system(Microsoft Windows系统进程)
 - tcpsvcs.exe(TCP/IP Services Application支持透过TCP/IP连接局域网和Internet)

2. 用户进程

3. 父进程
已创建一个或多个子进程的进程（Linux 中调用fork创建新进程的进程即为父进程）
4. 子进程
另一进程所创建的进程，子进程继承了对应的父进程的大部分属性，如文件描述符。在Unix中，子进程通常为系统调用fork的产物。在此情况下，子进程一开始就是父进程的副本，某一进程没有父进程，则可知该进程很可能由内核直接生成，进程ID为1的进程（即init进程）是在系统引导阶段由内核直接创建的。无父进程的进程，在对应的父进程结束执行后，进程就会变成孤儿进程，但之后会立即由init进程“收养”为其子进程。当某一子进程结束、中断或恢复执行时，内核会发送SIGCHLD信号予其父进程。在默认情况下，父进程会以SIG_IGN函数忽略之

## 进程的状态

### 三态模型

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-9-8/74729380.jpg)

就绪，运行，阻塞

- 就绪状态：当进程已分配到除CPU以外的所有必要的资源，只要获得处理机便可立即执行
- 运行状态：进程已获得处理机，其程序正在处理机上执行
- 阻塞状态：由于等待某个事件发生而无法执行时，便放弃处理机而处于阻塞状态(等待I/O完成、申请缓冲区不能满足、等待信件)

调度：时间片已经用完，让出处理器


### 五态模型

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-9-8/4195786.jpg)

### 进程状态转化

![](http://7xjl4u.com1.z0.glb.clouddn.com/15-9-8/78316643.jpg)


进程的控制机构是由操作系统内核实现，具体是通过控制原语。




## 原语 

若干条指令组成的，用于完成一定功能的一个过程，不可分割性.即原语的执行必须是连续的。
原语必须在管态下(cpu的特权态，执行指令系统的全集)执行，指令可以在目态(cpu的用户态，不能使用特权指令，不能直接取用系统资源与改变机器状态，并且只允许用户程序访问自己的存储区域)执行

### 包含

- 创建原语

实际系统中创建一个进程有两种方法：一是由操作系统建立，0#进程就是由操作系统建立的；二是由其他进程创建一个新的进程，创建进程原语总是先为新建进程申请一空白PCB，并为之分配唯一的数字表示符，使之获得PCB的内部名称，若该进程所对应的程序不在内存中，则应将它从外存储器调入内存，并将该进程有关信息填入PCB中，然后置该进程为就绪状态，并将它排入就绪队列和进程家族队列中，具体过程：1.从PCB集合种索取一个空白PCB，并获得该PCB的内部标识符i 2.把调用者提供的参数，以及从执行过程EP中获得的调用者内部标识j ，填入该PCB，设置记帐数据，置新进程为“静止就绪”状态 3.PCB分别插入就绪队列RQ和进程家族中

```
Procedure Create(n,S0,k0,M0,R0,acc)
begin
i:=get Internal Name(n);//获得内部名
i.id:=n;                //填外部名
i.Priority:=k0;         //填优先表
i.cpu state:=S0;        //填cpu初始状态
i.mainstore:=M0;        //填写主存区域
i.resource:=R0;         //填写资源清单
i.status:='Readys';     //填写进程状态
j:=EP;                  //获取调用者内部标识
i.Parent:=j;            //填入i进程的父进程j        
i.progeny:=null;        //i的家庭指针为空
j.progeny:=i;           //把i填入其父进程pcb中的家庭指针处
i.state:=RQ;            //i所在状态队列首指针
Insert(RQ,i);           //把i进程插入RQ队尾
continue;
end
```


- 撤销原语

撤消进程的实质是撤消进程存在标志——进程控制块PCB。一旦PCB被撤消，进程就消亡了，撤消进程的实质是撤消进程存在标志——进程控制块PCB。一旦PCB被撤消，进程就消亡了。撤消原语的操作过程大致如下：以调用者提供的标志符n为索引，从该进程所在的队列，将它从该队列中消去，并撤消属于该进程的一切“子孙进程”，若有父进程则从父进程PCB中删除指向该进程的指针，并释放撤消进程所占用的全部资源，或则将其归还给其父进程，或则归还给系统。若被撤消的进程处于执行状态，应立即中断该进程的执行，并设置调度标志为真，以指示该进程被撤消后系统应重新调度,撤销原语在撤销指定进程的同时也应该撤销所指示的子孙进程, 所以用kill()递归算法撤销子孙进程。引起进程终止事件可分三类：正常结束、异常结束、外界干预。
```
Procedure  destroy(n)
Begin
Sched:=false;
i:=getinternal name(n);//获取n进程的内部名
kill(i);
if sched then scheduler else continue;//假如Schde为真，则转调度程序，否则继续
end 
procedure kill(i)
begin
if i.stata(i)=”executing” then
begin stop(i);
sched:=true  end;
remove (i.stata,i);               //将被撤消进程从i.state所指示的队列中除去
for all S∈i.progeny do kill(s);
for all r∈(i.main store ∪i.resources) do
if owned(r) then insert (r.semaphore,r.date);//属于父进程的资源归还且插入父进程资源清单
for all ∈created resources(i) do remove descriptor (R);//撤消自己的资源清单归还系统
remove process control block (i);
end
```

- 阻塞原语

阻塞原语的大致工作过程如下：开始时，进程正处于执行状态，因此首先应中断CPU执行，并保存该进程的CPU现场，然后把阻塞状态赋予该进程，并将它插入到具有相同实体的阻塞队列中,引起阻塞和唤醒的事件可分四类：一、请求系统服务。二、启动某种操作（同步由int将该进程唤醒）三、新数据尚未到达时。四、无新工作可做。
```
Procedure  block
Begin
i:=EP;      //从执行进程EP获得调用者内部标识符
stop(i);
i.status:=”blockda”; i.stata=WQ(r);//填写阻塞队列指针
insert(WQ(r),i); //把i插入WQ队尾
scheduler        //转调度程序
end
```

- 唤醒原语

进程因为等待事件的发生而处于阻塞状态，当等待的事件完成后，进程又具有了继续执行的条件，这时就要把该进程从阻塞状态转变为就绪状态。这个工作由唤醒原语来完成。唤醒原语执行的操作有：先把被唤醒进程从阻塞队列中移出，设置该进程当前状态为就绪状态，然后再将该进程插入到就绪队列中。
```
Procedure wakeup(n)
Begin
i :=getinternal name(n);
remove (WQ(r),i);    //把i进程从等待r而受阻塞队列中摘除
i.status:=”ready”; //置i进程为“就绪”状态
i.sdata:=RQ;         //把i进程插入到就绪队列
insert(RQ,i);
continue
end
```

- 挂起原语

让某些进程暂时不参与资源竞争,挂起的方式有三类：一、把挂起原语调用者本身挂起，即自己挂起自己。二、挂起某个标识符的进程。三、将某个指定的标志符及其全部或部分子孙挂起用的保存n进程的pcb副本的内存区。 

```
Procedure suspend(n,a)
Begin
i:=getinternal name(n);
s:=i.status;
if s=”executing” then stop(i);
a:=copy PCB(i);
i.status:=if s=”blockeda” then “blockeds” else readys;
if s=”executing” then scheduler else continue;
end
```

- 激活原语

激活指定标志符的进程或激活某进程及其子孙进程

```
Procedure  active(n)
Begin
i:=getinternal name(n)
i.status:=if i.status=”readys” then “readya” else blockeda;
if i.status=”readys” then scheduler;//当激活后的进程处于“readys”状态针，将引起重新调度
else continue
end
```



### 分类

- 请求（Req）型 

高层向低层请求某种业务

- 证实（Cfm）型

提供业务的层证实某个动作已经完成

- 指示（Ind）型

提供业务的层向高层报告一个与特定业务相关的动作

- 响应(Res)型

用于应答，表示来自高层的指示原语已收到

---

感谢您的耐心阅读，如果您发现文章中有一些没表述清楚的，或者是不对的地方，请给我留言，你的鼓励是作者写作最大的动力，
如果您认为本文质量不错，读后觉得收获很大，不妨小额赞助我一下，让我更有动力继续写出高质量的文章。

- 支付宝 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/18963137.jpg)
- 微信 
![](http://7xjl4u.com1.z0.glb.clouddn.com/15-10-14/34122370.jpg)
   
作 者 : [@mousycoder](http://weibo.com/mousycoder)

原文出处 : http://mousycoder.com/2015/10/14/the-pragmatic-sa-13/

创作时间：2015-9-1

更新时间：2015-10-16


















