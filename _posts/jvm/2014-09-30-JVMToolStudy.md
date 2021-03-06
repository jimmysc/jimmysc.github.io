---
layout: post
category : jvm
tagline: "学习本文中JVM工具，协助定位问题"
tags : [jstat,mmap,top,jstack]
---
{% include JB/setup %}


## jstat
**用处:** 实时统计GC的时间，次数等，帮助识别应用是否存在频繁创建对象，但是未及时释放，导致频繁的GC。
    
	jstat -help
	jstat -options
	jstat -snap <pid>
	jstat -<option> [-t] [-h<lines>] <pid> [<mill second> [<count>]]


## jmap
**用处:** 用于dump堆内存进行分析，或者dump出堆内存中的对象明细，分析目前堆中对象占用的情况

	jmap -help
	jmap -dump
	jmap -histo

## top
**用处:** 实时显示linux中处理器的使用情况，内核管理的任务，及CPU，内存的使用率

10进制转成16进制方法：

	 echo 'ibase=10;obase=16;3845' | bc

## jstack
打印进程当前时刻的线程栈，可以分析是否发生了死锁，当前所有线程空忙情况，忙于做什么任务，同时可以定位消耗CPU的线程此时此刻的任务情况

[1.线程处理慢](https://github.com/jimmysc/jimmysc.github.com/tree/master/jimmysc-thread/src/main/java/com/jimmysc/thread/busy)

###### 1.1 运行测试
	1.运行BusyThreadTest
	2.jps
	3.jstack -l <pid>

###### 1.2 线程内容
	"BusyThread-99" prio=5 tid=0x00007fd2d2839000 nid=0x11603 waiting on condition [0x000000011e4d7000]
	   java.lang.Thread.State: TIMED_WAITING (sleeping)
		at java.lang.Thread.sleep(Native Method)
		at com.jimmysc.thread.busy.BusyThread1.readFileFormRemote(BusyThread1.java:18)
		at com.jimmysc.thread.busy.BusyThread1.run(BusyThread1.java:13)
		at java.lang.Thread.run(Thread.java:744)

	   Locked ownable synchronizers:
		- None

	"BusyThread-98" prio=5 tid=0x00007fd2d2838000 nid=0x11403 waiting on condition [0x000000011e3d4000]
	   java.lang.Thread.State: TIMED_WAITING (sleeping)
		at java.lang.Thread.sleep(Native Method)
		at com.jimmysc.thread.busy.BusyThread1.readFileFormRemote(BusyThread1.java:18)
		at com.jimmysc.thread.busy.BusyThread1.run(BusyThread1.java:13)
		at java.lang.Thread.run(Thread.java:744)


###### 1.3 运行结果
	所有的线程都在执行com.jimmysc.thread.busy.BusyThread1.readFileFormRemote(BusyThread1.java:18)
	说明该方法处理速度过慢，这种情况下会整个应用的线程逐步被耗光，导致应用的处理能力急剧下降


[2.死锁发生](https://github.com/jimmysc/jimmysc.github.com/tree/master/jimmysc-thread/src/main/java/com/jimmysc/thread/deadlock)

###### 2.1 运行测试

	1.运行ThreadDeadLockTest
	2.JPS
	3.jstack -l <pid>

###### 2.2 线程内容

	"Thread-0" prio=5 tid=0x00007f94f3832000 nid=0x5003 waiting for monitor entry [0x000000011972f000]
	java.lang.Thread.State: BLOCKED (on object monitor)
	at com.jimmysc.thread.deadlock.Thread1.run(Thread1.java:19)
	- waiting to lock <0x00000007d57a7008> (a java.lang.Class for com.jimmysc.thread.deadlock.ThreadLock2)
	- locked <0x00000007d5655148> (a java.lang.Class for com.jimmysc.thread.deadlock.ThreadLock1)
	at java.lang.Thread.run(Thread.java:744)
	
	Locked ownable synchronizers:
	- None
	线程有3部分信息
	1.线程的状态：waiting for monitor entry
	2.线程的调用栈：
	3.线程当前锁住的资源：locked <0x00000007d5655148> (a java.lang.Class for com.jimmysc.thread.deadlock.ThreadLock1)

###### 2.3 线程状态
	
	1.runnable
		表示线程具备运行条件等待操作系统调度，或正在运行中
	2.waiting on condition
		表示线程等待某个条件的发生：比如sleep、网络空闲等
	3.Waiting for monitor entry 和 in Object.wait() 
		Monitor是java中实现线程之间互斥和协作的主要手段，它可以看成是对象或class的锁，每个对象都有，但只有一个Monitor，下图描述Monitor，线程及线程状态的转换图

![](https://github.com/jimmysc/jimmysc.github.io/raw/master/images/jvm/JavaMonitor.png)

	任何时刻，每个 Monitor只能被 “Active Thread”拥有。其它线程都是 “Waiting Thread”，分别在两个队列 “ Entry Set”和 “Wait Set”里面等候。在 “Entry Set”中等待的线程状态是 “Waiting for monitor entry”，在 “Wait Set”中等待的线程状态是 “in Object.wait()”
	当执行到synchronized块的代码时（临界区），线程进入Entry Set队列中，若此时Monitor不被其他线程占用，并且Entry Set中没有其他线程，则当前线程会成为Monitor的Owner，执行synchronized中的代码，否则会一直在Entry Set等待；如果线程调用wait方法，则线程进入Wait Set等待，除非线程调用了对象的notify() 或 notifyAll()，Wait Set中的线程才有机会去竞争Monitor，但最终只有一个线程获得Monitor

###### 2.4 运行结果
		"Thread-1" prio=5 tid=0x00007f94f3832800 nid=0x5203 waiting for monitor entry [0x0000000119832000]
	   java.lang.Thread.State: BLOCKED (on object monitor)
		at com.jimmysc.thread.deadlock.Thread2.run(Thread2.java:19)
		- waiting to lock <0x00000007d5655148> (a java.lang.Class for com.jimmysc.thread.deadlock.ThreadLock1)
		- locked <0x00000007d57a7008> (a java.lang.Class for com.jimmysc.thread.deadlock.ThreadLock2)
		at java.lang.Thread.run(Thread.java:744)

	   Locked ownable synchronizers:
		- None

	"Thread-0" prio=5 tid=0x00007f94f3832000 nid=0x5003 waiting for monitor entry [0x000000011972f000]
	   java.lang.Thread.State: BLOCKED (on object monitor)
		at com.jimmysc.thread.deadlock.Thread1.run(Thread1.java:19)
		- waiting to lock <0x00000007d57a7008> (a java.lang.Class for com.jimmysc.thread.deadlock.ThreadLock2)
		- locked <0x00000007d5655148> (a java.lang.Class for com.jimmysc.thread.deadlock.ThreadLock1)
		at java.lang.Thread.run(Thread.java:744)

	   Locked ownable synchronizers:
		- None	
	
	1).Thread-1和Thread-0的线程状态都是waiting for monitor entry，表示都是在Entry Set队列中，都在等待拿到Monitor，多执行几次jstack -l <pid> 线程栈的信息都一样，说明无法拿到monitor；
	2).线程栈的提示信息显示BLOCKED，同时Thread-0在waiting to lock <0x00000007d57a7008> ；Thread-0又locked <0x00000007d5655148> ，Thread-1在waiting to lock <0x00000007d5655148>，locked <0x00000007d57a7008>，都锁住对方需要的锁，同时又等待对方释放锁，造成了死锁


## tsar

http://tsar.taobao.org/
