---
title: Java多线程
date: 2022-03-16 11:54:35
author: Evan
categories: Java基础
index_img: /img/bg3.jpg
tags:
- Java
- 多线程

---

# Java多线程

## 1 线程简介

案例:英雄联盟，微信聊天。

多任务：边吃饭比那玩手机，边开车边打电话，但同一瞬间依旧只做了一件事。

多线程：多条路解决阻塞问题。王者荣耀。

### 1.1 普通方法调用和多线程

![image](/img/java%E5%A4%9A%E7%BA%BF%E7%A8%8B/pic1.PNG)

* 进程：操作系统中运行的程序
* 线程：一个进程里面可以有多个线程，视频中，同时有声音，图像等



### 1.2 Process与Thread

* 说起线程，就不得不说下程序，程序时指令和数据的有序集合，其本身没有任何运行的含义，是一个静态的概念。

* 而**进程**则是执行程序的一次执行过程，它是一个动态的概念。时系统资源分配的单位

* 通常在一个进程中可以包若干个线程，当然一个进程至少有一个线程，不然没有存在的意义。线程时CPU调度和执行的单位。

  注意：很多多线程是模拟出来的，真正的多线程是指多个cpu，即多核，如服务器。
  如果是模拟出来的多线程，即在一个cpu的情况下，在同一个时间点，cpu只能执行一个代码，因为切换的很快，所以就有同时执行的错局。

* 线程就是独立的执行路径；
* 在程序运行时，即使没有直接创建线程，后台也会有多个线程，如主线程，gc线程；
* main()称之为主线程，为系统的入口，用于执行整个程序；
* 在一个进程中，如果开辟了多个线程，线程的运行由调度器安排调度，调度器是与操作系统紧密相关的，先后顺序是不能人为的干预的。
* 对于同一份资源的操作时，会存在资源抢夺的问题，需要加入并发控制；
* 线程会带来额外的开销，如cpu调度时间，并发控制开销。
* 每个线程在自己的工作内存交互，内存控制不当会造成数据不一致。

***

## 2 线程创建

### 2.1 继承Thread类

* 自定义线程类继承Thread类

* 重写run()方法，编写线程执行体

* 创建线程对象，调用start()方法启动线程

  ```java
  public class startThread1  extends Thread{
  	
  	public void run() {
  		for(int i=0;i<10;i++) {
  		System.out.println("我在上班");
  		}
  	}
  	public static void main(String[] args) {
  		startThread1 thread=new startThread1();
  		thread.start();
  		for(int i=0;i<10;i++) {
  			System.out.println("我在摸鱼");
  			}
  		
  	}
  }
  ```

  ### 2.2 实现Runnable接口（最核心)

* 实现MyRunnable类实现Runnable接口

* 实现run()方法，编写线程执行体

* 创建线程对象，调用start()方法启动线程

```java
public class startThread2 implements Runnable{

	public static void main(String[] args) {
		startThread2 t2=new startThread2();
		Thread thread=new Thread(t2);
		thread.start();
		for(int i=0;i<10;i++) {
			System.out.println("我在摸鱼");
		}
	}
	
	@Override
	public void run() {
		for(int i=0;i<10;i++) {
			System.out.println("我在上班");
		}
		
	}

}
```

**推荐使用Runnable对象，因为Java单继承的局限性**

* 继承Thread类
    * 子类继承Thread类具备多线程能力
    * 启动线程：子类对象.start()
    * 不建议使用:避免OOP单继承局限性
* 实现Runnable接口
  * 实现接口Runnable具有多线程能力
  * 启动线程：传入目标对象+Thread对象.start()
  * 推荐使用：避免单继承局限性，灵活方便，方便同一对象被多个线程使用

案例:龟兔赛跑

```java
/**  
 * 1.首先来个赛道距离，然后要离终点越来越近
 * 2.判断比赛是否结束
 * 3.打印出胜利者
 * 4.龟兔赛跑开始
 * 5.故事中乌龟赢的，兔子需要睡觉，所以我们来模拟兔子睡觉
 * 6.终于，乌龟赢得比赛
 */
public class Race implements Runnable {  
 /**胜利者*/  
 private String winner;  
 @Override  
 public void run() {  
     for (int i = 0; i <= 1000; i++) {  
         if (getOver(i)) {  
             break;  
         }  
        //兔子轻敌，睡觉去了 
         if(Thread.currentThread().getName().equals("兔")&&i==50){  
             try {  
                 Thread.sleep(10);  
                 System.out.println("兔子睡着了!");  
                 } 
             catch (InterruptedException e) {  
                 e.printStackTrace();  
             }  
         } 
         System.out.println(Thread.currentThread().getName()+"跑了"+i+"步;");  
     }  
 }  
 /**判断比赛是否结束*/  
 public boolean getOver(int i){  
     if(winner!=null){  
         return true;  
     }else {  
         if(i==100){  
             winner = Thread.currentThread().getName();  
             System.out.println(winner+"赢得了比赛!");  
             return true;  
         }  
     } 
     return false;  
 }  
 public static void main(String[] args) {  
     Race race = new Race();  
     new Thread(race,"兔").start();  
     new Thread(race,"龟").start();  
 }  
}
```



###  2.3 实现Callable接口

1.实现Callable接口，需要返回值类型

2.重写call方法，需要抛出异常

3.创建目标对象

4.创建执行服务：`ExecutorService ser=Executors.newFixedThreadPool(1);`

5.提交执行：`Future<Boolean> result1=ser.submit(t1);`

6.获取结果：`boolean r1 =result1.get()`

7.关闭服务：`ser.shutdownNow();`



用 Callable 接口改造下载图片案例

```java
import org.apache.commons.io.FileUtils;
import java.io.File;
import java.io.IOException;
import java.net.URL;
import java.util.concurrent.*;
//创建线程3 实现Callable 接口
/*
callable 的好处
1、可以定义返回值
2、可以抛出异常
 */
public class TestCallable implements Callable<Boolean> {
    private String url; //网络文件路径
    private String name;// 需要保存为什么文件名称
    public  TestCallable(String url,String name){
        this.url = url;
        this.name = name;
    }
    public Boolean call() {
        webDownLoader webDownLoader = new webDownLoader();
        webDownLoader.downLoader(url,name);
        System.out.println("下载了文件 ："  + name);
        return true;
    }
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        TestCallable thread01 = new TestCallable("图片路径","1.jpg");
        TestCallable thread02 = new TestCallable("图片路径","2.jpg");
        TestCallable thread03 = new TestCallable("图片路径","3.jpg");
        //创建执行服务:  3 条线程
        ExecutorService ser = Executors.newFixedThreadPool(3);
        //提交执行:
        Future<Boolean> result1 = ser.submit(thread01);
        Future<Boolean> result2 = ser.submit(thread02);
        Future<Boolean> result3 = ser.submit(thread03);
        //获取结果: 就是call 方法 返回的值
        boolean r1 = result1.get();
        boolean r2= result2.get();
        boolean r3 = result3.get();
        //关闭服务
        ser.shutdownNow();
    }
}
//下载器
class webDownLoader{
    //下载方法
    public  void downLoader(String url, String name){
        try {
            FileUtils.copyURLToFile(new URL(url),new File(name));
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("IO downLoader 方法异常。");
        }
    }
}
```

***

## 3 线程状态

![](/img/java多线程/pic2.PNG)

![](/img/java多线程/pic3.PNG)

![](/img/java多线程/pic4.PNG)

### 3.1 停止线程

* 不推荐使用JDK提供的stop()、destroy()方法。【已废弃】
* 推荐线程之间停止下来
* 建议使用一个标志位进行终止变量

当flag=false，则终止线程运行。

```java
public class TestStop implements Runnable {
	//1.线程中定义线程体使用的标识
	private boolean flag =true;
	
	public void run() {
		//2.线程体使用该标识
		while (flag) {
			System.out.println("run...Thread");
		}
	}
	//3.对外提供方法改变标识
	public void stop() {
		this.flag=false;
	}
}
```

***



## 4 线程休眠

* sleep(时间)指定当前线程阻塞的毫秒数；
* sleep存在异常InterrupterException；
* sleep时间达到后线程进入就绪状态；
* sleep可以模拟网络延时，倒计时等。
* 每一个对象都有一个锁，sleep不会释放锁；

网络延时的作用：放大问题的发生性

```java
public class TestSleep {
    public static void main(String[] args) {
        try {
            tenDown();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    public static void tenDown() throws InterruptedException {
        int num = 10;
        while (true){
            Thread.sleep(1000);
            System.out.println(num--);
            if(num<=0){
                break;
            }
        }
    }
}
```



***



## 5 线程礼让

* 礼让线程，让当前正在执行的线程暂停，但不阻塞
* 将线程从运行状态转为就绪状态
* 让cpu重新调度，礼让不一定成功！看CPU心情

例如：CPU正在运行A线程，此时A调用Thread.yield，变为就绪状态，Cpu重新调度A、B线程，

可能还是接着执行A线程，也有可能礼让成功，执行B线程。

```java
public class TestYield {
    public static void main(String[] args) {
        MyYield myYield = new MyYield();
        new Thread(myYield,"a").start();
        new Thread(myYield,"b").start();
    }
}
class MyYield implements Runnable{
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+"线程开始执行");
        Thread.yield();
        System.out.println(Thread.currentThread().getName()+"线程停止执行");
    }
}
//a线程开始执行
//b线程开始执行
//b线程停止执行
//a线程停止执行
```



***

## 6 线程强制执行

* Join合并线程，待此线程执行完成后，再执行其他线程，其他线程阻塞
* 可以想象成插队

```java
public class TestJoin implements Runnable{
	public static void main(String[] args) throws InterruptedException {
		TestJoin testJoin=new TestJoin();
		Thread thread=new Thread(testJoin);
			thread.start();
			for(int i=0;i<100;i++) {
				if(i==50) {
					thread.join();
				}
				System.out.println("main--"+i);
			}
	}

	@Override
	public void run() {
		for(int i=0;i<1000;i++) {
			System.out.println("join---"+i);
		}
	}
}
```

***



## 7 观测线程状态

`Thread.State`

 线程状态。线程可以处于一下状态之一：

* NEW

  尚未启动的线程处于此状态

* RUNNABLE

  在Java虚拟机中执行的线程处于此状态。

* BLOCKED

  被阻塞等待监视器锁定的线程处于此状态。

* WAITING

   正在等待另一个线程执行特定动作的线程处于此状态。

* TIMED_WAITING

  正在等待另一个线程执行动作达到指定等待时间的线程处于此状态。

* TERMINATED

  已退出的线程处于此状态。

一个线程可以在给定时间点处于一个状态。这些状态是不反映任何操作系统线程状态的虚拟机状态。



```java
public class MyThreadState implements Runnable {  
     @Override  
     public void run() {  
         for (int i = 0; i < 10; i++) {  
             if(i==5 && i<10){  
                 try {  
                     Thread.sleep(1);  
                     System.out.println("阻塞时的状态"+Thread.currentThread().getState());  
                 } catch (InterruptedException e) {  
                     e.printStackTrace();  
                  }  
             } 
             System.out.println(Thread.currentThread().getName()+i+"线程正在执行的状态："+Thread.currentThread().getState());  
         }  
     }
}
```

```java
public static void main(String[] args) {  
 MyThreadState myThreadState = new MyThreadState();  
    Thread thread = new Thread(myThreadState);  
    Thread.State state = thread.getState();  
    System.out.println("线程创建未启动时状态:"+state);  
    thread.start();  
    state = thread.getState();  
    System.out.println("线程启动后的状态:"+state);  
    while (thread.getState()!= Thread.State.TERMINATED){  
        state = thread.getState();  
        System.out.println(thread.getName()+"线程运行时状态:"+state);  
    }  
    state = thread.getState();  
    System.out.println("线程结束状态："+state);  
    System.out.println(Thread.currentThread().getName()+"线程优先级"+Thread.currentThread().getPriority());  
}
```

****



## 8 线程优先级

* Java提供一个线程调度器来监控程序中启动后进入就绪状态的所有线程，线程调度器按照优先级决定应该调度哪个线程来执行。
* 线程的优先级用数字表示，范围从1~10.
  * Thread.MIN_PRIORITY = 1 ;
  * Thread.MAX_PRIORITY = 10 ;
  * Thread.NORM_PRIORITY = 5 ;

* 使用以下方式改变或获取优先级
  * getPriority().setPriority(int xxx)

```java
class TestPriority extends Thread{  
     @Override  
     public void run() {  
     System.out.println(TestPriority.currentThread().getName()+"的优先级"+TestPriority.currentThread().getPriority());  
     }  
}
```

```java
public class MyThreadPriority {  
     public static void main(String[] args) {  
        TestPriority priority1 = new TestPriority();  
        TestPriority priority2 = new TestPriority();  
        TestPriority priority3 = new TestPriority();  
        TestPriority priority4 = new TestPriority();  
        TestPriority priority5 = new TestPriority();  
        System.out.println(Thread.currentThread().getName()+"线程优先级"+Thread.currentThread().getPriority());  
        priority1.setPriority(6);  
        priority1.start();  
//        System.out.println("priority1"+priority1.getPriority());  
        priority2.setPriority(2);  
        priority2.start();  
//        System.out.println("priority2"+priority2.getPriority());  
        priority3.setPriority(3);  
        priority3.start();  
//        System.out.println("priority3"+priority3.getPriority());  
        priority4.setPriority(8);  
        priority4.start();  
//        System.out.println("priority4"+priority4.getPriority());  
        priority5.setPriority(5);  
        priority5.start();  
//        System.out.println("priority5"+priority5.getPriority());  
     }  
}
```



***

## 9 守护线程

* 线程分为用户线程和守护线程
* 虚拟机必须确保用户线程执行完毕
* 虚拟机不用的等待守护线程执行完毕
* 如，后台记录操作日志，监控内存，垃圾回收等

```java
/**  
 * 上帝，守护线程  
 */  
class God implements Runnable{  
     @Override  
     public void run() {  
         long l = 0;
         while (true){  
             out.println("上帝保佑你！"+l++);  
         }  
     }
}
```

```java
/**  
 * you 用户线程  
 */  
class You implements Runnable{  
     @Override  
     public void run() {  
         for (int i = 0; i < 35000; i++) {  
             out.println("每天都在健康的活着"+i);  
         }  
         out.println("you goodBye world!");  
     }  
}
```

```java
/**
* 启动用户线程和守护线程
*/
public class MyThreadDaemon {  
     public static void main(String[] args) {  
        You you = new You();  
        God god = new God();  
        Thread youThread = new Thread(you);  
        Thread godThread = new Thread(god);  
        godThread.setDaemon(true);  
        youThread.start();  
        godThread.start();  
     }  
}
```

***

## 10 线程同步

多个线程操作同一资源。

### 10.1并发

同个对象 被多个线程 同时操作

* 现实生活中，我们会遇到“同个资源，多个人都想使用”的问题，比如，食堂排队打饭，每个人都想吃饭，最天然的结局方法就是，排队。一个个来。
* 处理多线程问题时，多个线程访问同一个对象，并且某些线程还想修改这个对象。这时候我们就需要线程同步。线程同步其实就是一种等待机制，多个需要同时访问此对象的线程进入这个对象的等待池形成队列，等待前面线程使用完毕，下一个线程再使用

**保证线程同步的安全性：队列加锁**

* 由于同一进程的多个线程共享同一块存储空间，在带来方便的同时，也带来了访问冲突问题，为了保证数据在方法中被访问时的正确性，在访问时加入**锁机制synchronized**，当一个线程获得对象的排它锁，独占资源，其他线程必须等待，使用后释放锁即可，存在以下问题：
  * 一个线程持有锁会导致其他所有需要此锁的线程挂起；
  * 在多线程竞争下，加锁，释放锁会导致比较多的上下文切换和调度延时，引起性能问题；
  * 如果一个优先级高的线程等待一个优先级低的线程释放锁会导致优先级倒置，引起性能问题。

### 10.2不安全线程

ArrayList是线程不安全的

```java

public class MyThreadSafe {  
    public static void main(String[] args) {  
        ArrayList<String> arrayList = new ArrayList<>();  
           for (int i = 0; i < 10000; i++) {  
                new Thread(()-> {  
                synchronized(arrayList){  
                arrayList.add(Thread.currentThread().getName());  
                               }  
                }).start();  
           }  
        try {  
            Thread.sleep(100);  
        } catch (InterruptedException e) {  
            e.printStackTrace();  
        }  
        System.out.println(arrayList.size());  
   }  
}

```

### 10.3 同步方法

* 由于我们可以通过private关键字来保证数据对象只能被方法访问，所以我们只需要针对方法提出一套机制，这套机制就是**synchronized**关键字，它包括两种用法：synchronized方法和synchronized块。

  **同步方法：`public synchronized void method(int args){}`**

* synchronized方法控制对"对象"的访问，每个对象对应一把锁，每个synchronized方法都必须获得调用该方法的对象的锁才能执行，否则线程会阻塞，方法一旦执行，就独占该锁，直到该方法返回才释放锁，后面被阻塞的线程才获得这个锁，继续执行

**缺陷：若将一个大的方法声明为synchronized将会影响效率**

* 方法里面需要修改的内容才需要锁，锁太多会浪费资源。

 synchronized默认是锁this，同步方法锁不住的时候只能用同步块

* 同步块:synchronized(Obj){}

* Obj称之为同步监视器

  * Obj可以是任何对象，但推荐使用共享资源作为同步监视器
  * 同步方法中无需指定同步监视器，因为同步方法的同步监视器就是this，就是这个对象本身，或者是class【反射】

* 同步监视器的执行过程

  1. 第一个线程访问，锁定同步监视器，执行其中代码。

  2. 第二个线程访问，发现同步监视器被锁定，无法访问。
  3. 第一个线程访问完毕，解锁同步监视器。
  4. 第二个线程访问，发现同步监视器没有锁，然后锁定并访问

锁要锁对，默认锁this，银行取钱，锁银行没有用，要锁银行卡。

锁要锁增删改的对象，同步块放在run()方法中。

**银行取钱问题**

1.账户类

```java
class Account {  
    /**余额*/  
    private int money;  
       /**卡名*/  
    private String name;  
    public Account(int money, String name) {
		this.money=money;
		this.name=name;
	}

	public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
    public int getMoney() {
        return money;
    }

    public void setMoney(int money) {
        this.money = money;
    }
}
```

2.模拟银行取钱

```java
/**  
 * 模拟银行取钱  
 */  
class Drawing extends Thread{  
     private final Account account;  
     /**现有的钱*/  
     private  int nowMoney;  
     /**取出的钱*/  
     private int drawingMoney;  
     public Drawing( Account account,  int drawingMoney,String name) {  
         super(name);  
         this.account = account;  
         this.drawingMoney = drawingMoney;  
        }  
     @Override  
     public void run() {  
         synchronized (account){  
         //判断有没有钱  
         if(account.getMoney()-drawingMoney<0){  
             out.println("你的账户没有那么多钱！");  
             return;  
         }try {  
             sleep(100);  
         } catch (InterruptedException e) {  
             e.printStackTrace();  
         }  
         //余额  
         account.setMoney( account.getMoney() - drawingMoney);  
         //手中的钱  
         nowMoney=nowMoney+drawingMoney;  
         out.println("余额为："+account.getMoney());  
         out.println(Thread.currentThread().getName()+"手中的钱："+nowMoney);  
        }  
     }
}
```

多人从银行取钱

```java
public class MyThread{  
 public static void main(String[] args) {  
     Account account = new Account(100, "工商");  
     Drawing me = new Drawing(account, 40, "me");  
     Drawing girlFriend = new Drawing(account, 89, "girlFriend");  
     me.start();  
     girlFriend.start();  
    }  
}
```

### 10.4 死锁

* 多个线程各自占有一些共享资源，并且互相等待其他线程占有的资源才能运行，而导致两个或者多个线程都在等待对方释放资源，都停止执行的情形，某一个同步块同时拥有“**两个以上对象的锁**”时，就可能会发生“死锁”的问题。
* 产生死锁的四个必要条件:

```
1.互斥条件：一个资源每次只能被一个进程使用。
2.请求与保持条件:一个进程因请求资源而阻塞时，对已获得的资源保持不放。
3.不剥夺条件：进程已获得的资源，在未使用完之前，不能强行剥夺。
4.循环等待条件：若干进程之间形成一种头尾相接的循环等待资源关系。
```

**只要想办法破坏上述任意一个或多个条件就可以避免死锁发生。**

实现一个死锁

```java
/**  
 * 口红  
 */  
class Lipstick{  
}  
/**  
 * 镜子  
 */  
class Mirroe{  
}
/**  
 * 写一个抢夺资源的方法  
 */  
public class MyThread{

	 public static void main(String[] args) {  
	       Makeup g1=new Makeup(0,"小红");
	       Makeup g2=new Makeup(1,"小丽");
	       g1.start();
	       g2.start();
	   }  
	
}
class Makeup extends Thread{  
    static Lipstick lipstick = new Lipstick();  
    static Mirroe mirroe = new Mirroe();  
    int choice;  
    String girlName;  
    public Makeup(int choice, String girlName) {  
        this.choice = choice;  
        this.girlName = girlName;  
    }  
     @Override  
     public void run() {  
         try {  
             makeup();  
         } catch (InterruptedException e) {  
             e.printStackTrace();  
         }  
     }  
     public void makeup() throws InterruptedException {  
         if(choice==0){  
             synchronized (lipstick){  
                 System.out.println(this.girlName+"拿到口红");  
                 Thread.sleep(1000);  
                 synchronized (mirroe){  
                     System.out.println(this.girlName+"拿到镜子");  
                 }  
             } 
         }else{  
             synchronized (mirroe){  
                 System.out.println(this.girlName+"拿到镜子");  
                 Thread.sleep(2000);  
                 synchronized (lipstick){  
                     System.out.println(this.girlName+"拿到口红");  
                 }  
             } 
         } 
     }
}
```



***

## 11 Lock锁

* 从JDK5.0开始，Java提供了更强大的线程同步机制——通过显式定义同步锁对象来实现同步。同步锁使用Lock对象充当
* `java.util.concurrent.locks.Lock`接口是控制多个线程对共享资源进行访问的工具。锁提供了对共享资源的独占访问，每次只能有一个线程对Lock对象加锁，线程开始访问共享资源之前应先获得Lock对象
* ReentrantLock类实现了Lock，它拥有与synchronized相同得并发性和内存语义，在实现线程安全得控制中，比较常用得是ReentrantLock，可以显式加锁、释放锁。

```java
//实现
class A{
     private final ReentrantLock lock = new ReentrantLock();  
    public void m(){
        lock.lock();
        try{
            //保证线程安全的代码；
        }
        finally{
            lock.unlock();
            //如果同步代码有异常，要将unlock()写入finally语句块
        		}
    	}
    }
```

多人买票例子：

```java
/**  
 * 多人买票，用lock的方式实现  
 */  
class TestLock implements Runnable{  
    int ticketNums=100;  
    private final ReentrantLock lock = new ReentrantLock();  
     @Override  
     public void run() {  
         while (true){ 	
             try {  
            	 lock.lock(); 
                 if(ticketNums>0){  
                     try {  
                    	    Thread.sleep(1000);
                      } catch (InterruptedException e) {  
                         e.printStackTrace();  
                      }  
                 System.out.println(Thread.currentThread().getName()+"买到了票"+ticketNums--);  
                  }else {  
                     break;  
                  }  
              }finally {  
                  lock.unlock();  
              
              }  
            
         } 
     }
}
public class MyLock {  
    public static void main(String[] args) {  
        TestLock lock = new TestLock(); 
        new Thread(lock,"彭于晏").start();  
        new Thread(lock,"陈冠希").start();  
        new Thread(lock,"Evan").start();  
    }  
}
```

### 11.1 Lock和synchronized对比

* Lock是显式锁（手动开启和关闭锁，别忘记关闭锁）synchronized是隐式锁，出了作用域自动释放
* Lock只有代码块锁，synchronized有代码块锁和方法锁
* 使用Lock锁，JVM将花费较少的时间来调度线程，性能更好。并且具有更好的扩展性（提供更多的子类）
* 优先使用顺序：
  * Lock > 同步代码块（已经进入了方法体，分配了相应资源） >同步方法（在方法体之外）

```java
// 定义可重入锁
private final ReentrantLock lock = new ReentrantLock();
// 加锁
lock.lock();
 // 解锁         
lock.unlock();    
```

***

## 12 线程协作

### 12.1生产者消费者模式

* 应用场景：生产者和消费者问题
  - 假设仓库中只能存放一件产品,生产者将生产出来的产品放入仓库,消费者将仓库中产品取走消费.
  - 如果仓库中没有产品﹐则生产者将产品放入仓库，否则停止生产并等待，直到仓库中的产品被消费者取走为止.
  - 如果仓库中放有产品﹐则消费者可以将产品取走消费，否则停止消费并等待，直到仓库中再次放入产品为止。

![](/img/java%E5%A4%9A%E7%BA%BF%E7%A8%8B/pic5.png)

-  **这是一个线程同步问题，生产者和消费者共享同一个资源，并且生产者和消费者之间相互依赖,互为条件．**

  - 对于生产者﹐没有生产产品之前，要通知消费者等待﹒而生产了产品之后﹐又需要马上通知消费者消费

  - 对于消费者﹐在消费之后,要通知生产者已经结束消费﹐需要生产新的产品以供消费.

  - 在生产者消费者问题中,仅有synchronized是不够的

    * synchronized可阻止并发更新同一个共享资源，实现了同步

    * synchronized不能用来实现不同线程之间的消息传递(通信)

* Java提供了几个方法解决线程之间的通信问题

  ![](/img/java%E5%A4%9A%E7%BA%BF%E7%A8%8B/pic6.png)

**sleep 会抱着锁睡觉，wait 会释放锁**

**解决办法一**

并发协作模型“生产者l消费者模式”—->管程法

```tex
生产者:负责生产数据的模块(可能是方法﹐对象﹐线程,进程);
消费者:负责处理数据的模块(可能是方法﹐对象﹐线程﹐进程);
缓冲区:消费者不能直接使用生产者的数据﹐他们之间有个“缓冲区”
```

**生产者将生产好的数据放入缓冲区,消费者从缓冲区拿出数据**

例子：

```java
//测试：生产者消费者模型-->利用缓冲区解决：管程法
//生产者，消费者，产品，缓冲区
public class TestPC {
    public static void main(String[] args) {
        SynContainer container = new SynContainer();
        new Productor(container).start();
        new Consumer(container).start();
    }
}
//生产者
class Productor extends Thread{
    SynContainer container;
    public Productor(SynContainer container){
        this.container = container;
    }
    //生产
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            container.push(new Chicken(i));
            System.out.println("生产了"+i+"只鸡");
        }
    }
}
//消费者
class Consumer extends Thread{
    SynContainer container;
    public Consumer(SynContainer container){
        this.container = container;
    }
    //消费
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("消费了--->"+container.pop().id+"只鸡");
        }
    }
}
//产品
class Chicken{
    int id;// 产品编号
    public Chicken(int id){
        this.id = id;
    }
}
//缓冲区
class SynContainer{
    //需要一个容器大小
    Chicken[] chickens = new Chicken[10];
    //容器计算器
    int count = 0;
    //生产者放入产品
    public synchronized void push(Chicken chicken){
        //如果容器满了，就需要等待消费产品
        if(count==chickens.length){
            //生产者等待
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        //如果没有满，我们就需要丢入产品
        chickens[count] = chicken;
        count++;
        //可以通知消费者消费了
        this.notifyAll();
    }
    //消费者消费产品
    public synchronized Chicken pop(){
        //判断能否消费
        if (count == 0){
            //等待生产者生产，消费者等待
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        //如果可以消费
        count--;
        Chicken chicken = chickens[count];
        //吃完了，通知生产者生产
        this.notifyAll();
        return chicken;
    }
}
```



**解决办法二**

并发协作模型“生产者/消费者模式”—->信号灯法

```java
//测试消费者和生产者问题2：信号灯法
public class TestPC2 {
    public static void main(String[] args) {
        TV tv = new TV();
        new Player(tv).start();
        new Watcher(tv).start();
    }
}
//生产者-->演员
class Player extends Thread{
    TV tv;
    public Player(TV tv){
        this.tv = tv;
    }
    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            if (i%2==0){
                this.tv.play("快乐大本营播放中");
            }else{
                this.tv.play("抖音，记录有钱人的美好生活");
            }
        }
    }
}
//消费者-->观众
class Watcher extends Thread{
    TV tv;
    public Watcher(TV tv){
        this.tv = tv;
    }
    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            tv.watch();
        }
    }
}
//产品-->节目
class TV{
    // 演员表演，观众等待 T
    // 观众观看，演员等待 F
    String voice; // 表演的节目
    boolean flag = true;
    // 表演
    public synchronized void play(String voice){
        if (!flag){
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("演员表演了："+voice);
        // 通知观众观看
        this.notifyAll();// 通知唤醒
        this.voice = voice;
        this.flag = !this.flag;//flag如果是真就取假，相同是假就取真
    }
    // 观看
    public synchronized void watch(){
        if (flag){
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("观众观看了："+voice);
        // 通知演员表演
        this.notifyAll();
        this.flag = !this.flag;
    }
}
```

***

## 13 线程池

* 背景：经常创建和销毁、使用量特别大的资源，比如并发情况下的线程，对性能影响很大。
* 思路:  提前创建好多个线程，放入线程池中，使用时直接获取，使用完放回池中。可以避免频繁创建销毁、实现重复利用。类似生活中的公共交通工具。
* 好处:
  * 提高响应速度（减少了创建新线程的时间）
  * 降低了资源消耗（重复利用线程池中线程，不需要每次都创建）
  * 便于线程管理（...）
    * `corePoolSize`：核心池的大小
    * `maximumPoolSize`:最大线程数
    * `keepAliveTime`：线程没有任务时最多保持多长时间后会终止

### 13.1使用线程池

* JDK 5.0起提供了线程池相关API：**ExecutorService** 和**Executors**

* **ExecutorService**：真正的线程池接口。常见子类ThreadPoolExecutor

  * `void execute(Runnable command)`:执行任务/命令，没有返回值，一般哟过来执行Runnable
  * `<T> Future<T>submit(Callable<T>task)`: 执行任务，有返回值，一般用来执行Callable
  * `void shutdown() `:关闭连接池

* **Executors**：工具类、线程池的工厂类，用于创建并返回不同类型的线程池

  ```java
  // 1. 创建线程池, 参数为线程池大小 
  ExecutorService service = Executors.newFixedThreadPool(10);
  // 2. 添加线程
  service.submit(new MyThread()); 
  service.submit(new MyThread());
  service.submit(new MyThread());
  service.submit(new MyThread());
  // 3. 关闭线程池 
  service.shutdown();
  ```

  

***

本文完 

本文源自：https://www.bilibili.com/video/BV1V4411p7EF?spm_id_from=333.999.0.0

