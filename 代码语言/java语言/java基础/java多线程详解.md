# java多线程详解

## 参考

https://www.cnblogs.com/jimoer/p/13283355.html

https://juconcurrent.com/2018/10/26/wait-notify/



## 操作系统中的用户级线程和内核级线程

### 用户级线程(就是程序代码中完成某一特定功能的代码段，一个大程序中的一部分)

```
用户级线程就是我们编程中的一段代码，这段代码在逻辑上完成某项功能(比如处理视频聊天的功能)
```

![image-20210403125029832](https://raw.githubusercontent.com/yusenyi123/pictures2/master/imgs/20210403125037.png)



### 内核级线程(操作系统引入内核级线程，将除cpu外的资源分配和cpu的资源分配(cpu调度)独立开来,分别对应进程和线程)

```
在引入内核级线程后，进程是系统资源分配的基本单位(此处的系统资源不包括cpu资源，因为cpu资源由内核级线程占用)
内核级线程是cpu调度的基本单位(也就是内核级线程占用cpu资源)
一个进程可以包含多个内核级线程，这些线程共享进程的资源



```

### 在引入内核级线程后，又根据和用户代码中根据功能划分出来的用户级线程，两者的对应关系产生了多线程的三种模型

#### 一对一模型(一个用户级线程对应一个内核级线程)



#### 多对一模型(多个用户级线程对应一个内核级线程，这种情况就和原先没有引入内核级线程，只有进程时相同)





#### 多对多模型(n用户级线程映射到m个内核级线程(n>=m)。每个用户进程对应m个内核级线程。)

```
多对多模型可以这样理解
一个程序的某几个功能(一个功能就是一个用户级线程)对应一个内核级线程，另外的某几个功能又对应一个内核级线程

假设是一个单核cpu，那么同一时间只能由一个内核级线程运行，当这个内核级线程运行的时候有可能运行的是某一段功能对应的代码(也就是执行的是某个用户级线程)
```









## Java线程的实现(JDK1.2以前用户线程，JDK1.3后才有1对1模型，每个Thread对象都对应一个内核级线程)

Java线程如何实现并不受Java虚拟机规范约束，这是一个与具体虚拟机相关的画图。Java线程在早期的Classic虚拟机上（JDK1.2以前），是基于一种被称为“绿色线程”（Green Threads）的用户线程实现的，但从JDK1.3起，“**主流**”平台上的“**主流**”商用Java虚拟机的线程模型普遍都被替换为基于操作系统原生线程模型来实现，即采用1:1的线程模型。









### Java线程状态转换

Java语言定义了6种线程状态，在任意一个时间点钟，一个线程只能有且只有其中的一种状态，并且可以通过特定的方法在不同状态之间切换。

- **新建（New）**：创建后尚未启动的线程处于这种状态。
- **运行（Runnable）**：包括操作系统线程状态中的Running和Ready，也就是处理此状态的线程有可能正在执行，也有可能正在等待着操作系统为它分配执行时间。
- **无限期等待（Waiting）**：处于这种状态的线程不会被分配处理器执行时间，它们要等待被其他线程显示唤醒。
  以下方法会让线程陷入无限期等待状态：
  **1**、没有设置Timeout参数的`Object::wait()`方法；
  **2**、没有设置Timeout参数的`Thread::join()`方法；
  **3**、`LockSupport::park()`方法。
- **限期等待（Timed Waiting）**：处于这种状态的线程也不会被分配处理器执行时间，不过无须等待被其他线程显式唤醒，在一定时间之后它们会由系统自动唤醒。
  以下方法会让线程进入限期等待状态：
  **1**、`Thread::sleep()`方法；
  **2**、设置了Timeout参数的`Object::wait()`方法；
  **3**、设置了Timeout参数的`Thread::join()`方法；
  **4**、`LockSupport::parkNanos()`方法；
  **5**、`LockSupport::parkUntil()`方法；
- **阻塞（Blocked）**：线程被阻塞了，“阻塞状态”与“等待状态”的区别是“阻塞状态”在等待着获取到一个排他锁，这个事件将在另外一个线程放弃这个锁的时候发生；而“等待状态”则是在等待一段时间 ，或者唤醒动作发生。在程序进入同步区域的时候，线程将进入这种状态。
- **结束（Terminated）**：已终止线程的线程状态，线程已经结束执行。

![线程状态转换关系](https://img-blog.csdnimg.cn/20200711113431650.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_SmlNb2Vy,size_60,color_c8cae6,t_70)







## 学习知识点！！！

### *JUC*就是java.util .concurrent工具包的简称  concurrent(并发)



### 1.new Thread(Runnable接口实现类).start() 是一种静态代理的模式

```

```



### 2.为什么wait(),notify(),notifyAll()必须在同步（Synchronized）方法/代码块中调用？

```java
class BlockingQueue {
    Queue<String> buffer = new LinkedList<String>();

    public void give(String data) {
        buffer.add(data);
        notify();                   
    }

    public String take() throws InterruptedException {
        while (buffer.isEmpty())    
            wait();
        return buffer.remove();
    }
}
```

```
上述代码中，假设wait()和  notify()不需要在Synchronized代码块或Synchronized方法中，
那么假设有一个消费者线程执行take()时发现buffer.isempty()==true，刚判断完成后cpu就进行了调度，执行了一个生产者线程，执行了完notify()后再进行调度执行消费者线程

这样逻辑上就出现问题，buffer中已经存在数据了，但是消费者线程还在阻塞中


所以这里要保证判断操作和wait()操作是原子性的，所以要把判断和wait()一起放到Synchronized代码块或Synchronized方法中，这是java设计者在考虑多线程实际编程时考虑到的问题
wait/notify是线程之间的通信，他们存在竞态,竞态指当两个线程竞争同一资源时，如果对资源的访问顺序敏感


我们在使用wait()时必然是在等待某种条件的满足，而条件的满足又是另外一个线程在更改条件之后进行了notify()通知
所以wait()操作和判断条件应该是原子性的，不能被与之有影响的操作中断


```





表面上，如果`wait()`和`notify()`不放在synchroized中，执行时会抛出异常`IllegalMonitorStateException`，即：非法的监视器状态异常。

那么为什么会是这样的呢？主要原因还是因为存在`竟态条件`！这儿先科普一下，什么是`竟态条件`？

> 当两个线程竞争同一资源时，如果对资源的访问顺序敏感，就称存在竞态条件。

`wait()`的调用必然是在等待某种条件的满足，而条件的满足又是另外一个线程在更改条件之后进行了`notify()`通知，所以我们期望达到如下的效果：

```
// 生产者B
void produce() {
	修改条件使其满足
	notify();
	// do something
}

// 消费者A
void consume() {
	while (条件不满足) {
		wait();
	}
	// do something
}
```

如果不加`synchronized`，可能出现下面的情况

1. A进入`while循环`后，但还没有执行`wait()`方法
2. B更新了条件，通过`notify()`唤醒等待队列中的线程。这时因为A还没有执行到`wait()`方法，所以A不会被唤醒。
3. A执行到`wait()`了，如果除了A之后没有其他线程的`notify()`通知到A，那么A会一直保持在`WAITING`状态而得不到后续的执行

那么如果我们加上`synchronized`后，变成下面这种情况，又可不可以呢？

```
// 生产者B
void produce() {
	synchronized(obj_a) {
		修改条件使其满足
		obj_b.notify();
		// do something
	}
}

// 消费者A
void consume() {
	synchronized(obj_a) {
		while (条件不满足) {
			obj_b.wait();
		}
		// do something
	}
}
```

表面上看貌似可以，实际上却是行不通的。因为`wait()`在线程状态变更之前必然会释放锁，而sychronized可以嵌套包含，这时`wait()`就不知道应该释放哪个锁了。所以最理所当然地就是把`wait()`所属对象当做释放的锁，所以`JUC`的作者就使用了这样一种很直观，又很合理的方式。

最终的呈现如下：

```
// 生产者B
void produce() {
	synchronized(obj_a) {
		修改条件使其满足
		obj_a.notify();
		// do something
	}
}

// 消费者A
void consume() {
	synchronized(obj_a) {
		while (条件不满足) {
			obj_a.wait();
		}
		// do something
	}
}
```





### 3.Synchronized锁定的对象

```
1.Synchronized方法锁定this

2.Synchronized(静态变量) 锁定class对象

3.Synchronized(成员变量)  锁定成员变量
```

