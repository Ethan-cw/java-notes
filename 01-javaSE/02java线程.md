线程

三种创建方式：

* Thread class 继承thread类（重点）
* Runnable接口 实现Runnable接口（重点）
* Callable接口 实现Callable接口（了解）

## 方式一：重点，不建议用

1、自定义线程类

2、重写run()方法，编写线程执行体

3、创建线程对象，调用start()方法启动线程

不建议使用：避免OOP单继承局限性

```java
package com.thread;
/*
方式一：
1. 继承Thread类
2. 重写run()方法
3. 调用start开启线程
 */
public class TestThread1 extends Thread{

    @Override
    public void run() {
        //run方法线程体
        for (int i = 0; i < 200; i++) {
            System.out.println("我在看代码-----"+i);
        }
    }

    public static void main(String[] args) {
        //main线程, 主线程
        //创建一个线程对象
        TestThread1 testThread1 = new TestThread1();
        //调用start()方法开启线程 和主线程交替执行
        testThread1.start();
        
        for (int i = 0; i < 200; i++) {
            System.out.println("我在摸鱼-----"+i);
        }
    }
}
```

## 方式二：重点，推荐用

1. 定义MyRunnable类实现Runnable接口
2. 实现run()方法，编写线程执行体
3. 创建线程对象，调用start()方法启动线程

推荐使用Runnable对象，方便同一个对象被多个线程使用

```java
package com.thread;

/**
 * 创建线程方式2：
 * 1、实现runnable接口
 * 2、重写run()方法
 * 3、执行线程需要丢入runnable接口实现类，调用start方法
 */
public class TestThread2 implements Runnable {
    @Override
    public void run() {
        //run方法线程体
        for (int i = 0; i < 200; i++) {
            System.out.println("我在看代码-----" + i);
        }
    }

    public static void main(String[] args) {
        //创建runnable接口的实现类对象
        TestThread2 testThread2 = new TestThread2();
        //常见线程对象，通过线程对象来开启我们的线程，这个是代理

        //Thread thread = new Thread(testThread2);
        //thread.start();
        new Thread(testThread2).start();
        
        for (int i = 0; i < 200; i++) {
            System.out.println("我在摸鱼-----" + i);
        }
    }
}
```

## 初识多线程

```java
package com.thread;

//多个线程同时操作同一个对象
//买火车票的例子

//发现问题：多个线程操作同一个资源，并发问题
public class TestThread3 implements Runnable{

    //票数
    private int ticketNums = 10;

    @Override
    public void run() {
        while(true){
            if(ticketNums<=0) break;
            try {
                Thread.sleep(200);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName()+"-->拿到了第"+ticketNums--+"票");
        }
    }
```

## 经典案例龟兔赛跑

```java
package com.thread;
//模拟龟兔赛跑
public class Race implements Runnable{

    //胜利者
    private  static  String winner;

    @Override
    public void run() {
        for (int i = 0; i <= 100; i++) {
            //模拟兔子休息
            if (Thread.currentThread().getName().equals("兔子") && i%10==0){
                try {
                    Thread.sleep(200);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }

            boolean flag = gameOver(i);
            if (flag){
                break;
            }
            System.out.println(Thread.currentThread().getName()+"-->跑了"+i+"米");

        }
    }

    private  boolean gameOver(int steps){
        if (winner!=null){
            return true;
        }else {
            if(steps==100){
                winner=Thread.currentThread().getName();
                System.out.println("winner is "+ winner);
                return true;
            }
        }
        return false;
    }

    public static void main(String[] args) {
        Race race = new Race();

        new Thread(race,"乌龟").start();
        new Thread(race,"兔子").start();
    }
}
```

## 方式三：了解

1. 实现Callable接口，需要返回值类型
2. 重写call方法，需要抛出异常
3. 创建目标对象
4. 创建执行服务
5. 提交执行
6. 获取结果
7. 关闭服务

好处：可以定义返回值，可以抛出异常

```java
package com.thread;

import java.util.concurrent.*;

public class TestCallable implements Callable<Boolean> {

    //胜利者
    private  static  String winner;
    private  String runner;


    public TestCallable(String name) {
        this.runner = name;
    }

    @Override
    public Boolean call() {
        for (int i = 0; i <= 1000; i++) {
            boolean flag = gameOver(i);
            if (flag){
                break;
            }
            //模拟兔子休息
            if (this.runner.equals("兔子") && i%10==0){
                try {
                    Thread.sleep(200);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            //模拟乌龟休息
            if (this.runner.equals("乌龟") && i%10==0){
                try {
                    Thread.sleep(50);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            System.out.println(this.runner+"-->跑了"+i+"米");
        }
        return true;
    }

    private  boolean gameOver(int steps){
        if(this.winner!=null){
            return true;
        }
        if(steps==1000) {
            this.winner = this.runner;
            System.out.println("winner is " + this.winner);
            return true;
        }
        return false;
    }

    public static void main(String[] args) throws ExecutionException, InterruptedException {
        //创建目标对象
        TestCallable r1 = new TestCallable("兔子");
        TestCallable r2 = new TestCallable("乌龟");

        //创建执行服务
        ExecutorService ser = Executors.newFixedThreadPool(2);
        //提交执行
        Future<Boolean> result1 = ser.submit(r1);
        Future<Boolean> result2 = ser.submit(r2);

        //获取结果
        boolean rs1 = result1.get();
        boolean rs2 = result2.get();

        //关闭服务
        ser.shutdownNow();
    }
}
```

## 静态代理

```java
package com.thread;

/**
 * 静态代理模式总结：
 * 1.真实对象和代理对象都要实现同一个接口
 * 2.代理对象要代理真实角色
 *
 * 好处：
 * 代理对象可以做真实对象做不了的事情
 * 真实对象专注做自己的事情
 */
public class StacticProxy {
    public static void main(String[] args) {

        WeddingCompany weddingCompany = new WeddingCompany(new You("小明"));
        weddingCompany.happyMarry();
    }
}

interface Marry{
    void happyMarry();
}
//真实角色
class You implements Marry{
    private String name;

    public You(String name) {
        this.name = name;
    }

    @Override
    public void happyMarry() {
        System.out.println(this.name+"要结婚了");
    }
}
//代理角色，帮人结婚
class WeddingCompany implements Marry{
    //真实目标角色
    private Marry target;

    public WeddingCompany(Marry target) {
        this.target = target;
    }

    @Override
    public void happyMarry() {
        before();
        this.target.happyMarry();//真实对象调用结婚
        after();
    }

    private void after() {
        System.out.println("结婚后，收钱");
    }

    private void before() {
        System.out.println("结婚，帮布置现场");
    }
}
```

## Lambda表达式

```java
package com.lambda;
/*
推导lambda表达式
 */
public class TestLambda {

    //3. 静态内部类
    static class Like2 implements ILike{
        @Override
        public void lambda() {
            System.out.println("i like lambda2");
        }
    }

    public static void main(String[] args) {
        ILike like = new Like();
        like.lambda();

        ILike like2 = new Like2();
        like2.lambda();

        //4.局部内部类
        class Like3 implements ILike{
            @Override
            public void lambda() {
                System.out.println("i like lambda3");
            }
        }

        ILike like3 = new Like3();
        like3.lambda();

        //5. 匿名内部类 没有类的名称，必须借助接口或者父类
        ILike like4 = new ILike() {
            @Override
            public void lambda() {
                System.out.println("i like lambda4");
            }
        };
        like4.lambda();

        //6. 用lambda简化
        ILike like5 = () -> System.out.println("i like lambda5");
        like5.lambda();
    }
}

//1、定义一个函数式接口
interface ILike{
    void lambda();
}

//2.实现类
class Like implements ILike{
    @Override
    public void lambda() {
        System.out.println("i like lambda");
    }
}

```

## 线程的方法

### 1. 停止线程

不推荐用.stop()，.destroy()方法，已经废弃。

推荐线程自己停下来。建议使用标志位，进行终止变量。当flag=false，则终止线程运行。

```java
package com.thread.state;

//测试线程停止
//不推荐用.stop()，.destroy()方法，已经废弃。
//推荐线程自己停下来。建议使用标志位，进行终止变量。当flag=false，则终止线程运行。
public class TestStop implements Runnable{

    //1.设置一个标识位，默认为true
    private boolean flag = true;
    @Override
    public void run() {
        int i = 0;
        while (flag){
            System.out.println("Thread running" + i++);
        }
    }

    //2.设置一个公开的方法停止线程，转换标志位
    public void stop() {
        this.flag = false;
    }

    public static void main(String[] args) {
        TestStop testStop = new TestStop();
        new Thread(testStop).start();

        for (int i = 0; i < 1000; i++) {
            System.out.println("main"+i);
            if (i==900) {
                testStop.stop();
                System.out.println("线程停止了");
            }
        }
    }
}
```

### 2. 线程休眠 sleep

sleep(时间)指定当前线程阻塞的毫秒数；

sleep存在异常InterruptedException;

sleep时间到达后线程进入就绪状态；

sleep可以模拟网络延时，倒计时等。

每一个对象都有一个锁，sleep不会释放锁

```java
package com.thread.state;

import java.sql.Date;
import java.text.SimpleDateFormat;

//模拟倒计时
public class TestSleep2 {

    public static void main(String[] args) {
        //打印当前系统时间
        Date startTime = new Date(System.currentTimeMillis());//获取系统当前时间
        while(true){
            try {
                Thread.sleep(1000);
                System.out.println(new SimpleDateFormat("HH:mm:ss").format(startTime));
                startTime = new Date(System.currentTimeMillis());//更新时间
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        }
    }

    //模拟倒计时
    public static void tenDown() throws InterruptedException{
        int num=10;
        while(true){
            Thread.sleep(1000);
            System.out.println(num--);
            if (num<=0){
                break;
            }
        }
    }
}
```

### 3. 线程礼让 yield

让当前的线程暂停，但不阻塞

让线程从运行状态转为就绪状态

让CPU重新调度，礼让不一定成功，看cpu心情

```java
package com.thread.state;

//测试礼让
//礼让不一定成功
public class TestYield {
    public static void main(String[] args) {
        MyYield myYield = new MyYield();

        new Thread(myYield,"小明").start();
        new Thread(myYield,"小红").start();
    }
}


class MyYield implements Runnable{

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+"线程开始执行");
        Thread.yield();//线程礼让
        System.out.println(Thread.currentThread().getName()+"线程执行结束");
    }
}
```

### 4. 线程插队 Join 

该线程执行完毕再执行其他线程

```java
package com.thread.state;

public class TestJoin implements Runnable{

    @Override
    public void run() {
        for (int i = 0; i < 1000; i++) {
            System.out.println("vip线程"+i++);
        }
    }

    public static void main(String[] args) {

        //启动线程
        TestJoin testJoin = new TestJoin();
        Thread thread = new Thread(testJoin);
        thread.start();

        //主线程
        for (int i = 0; i < 500; i++) {
            if(i==200){
                try {
                    thread.join();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            System.out.println("main"+i++);
        }
    }
}
```

### 5. 线程状态

新生 就绪 运行 阻塞 死亡

![1637566213(1)](C:\Users\Ethan\Desktop\java\img\1637566213(1).png)

```java
package com.thread.state;

//观察测试线程的状态
public class TestState {

    public static void main(String[] args) {

        Thread thread = new Thread(()->{
            for (int i = 0; i < 5; i++) {
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            System.out.println("---------------------------");
        });

        //观察状态
        Thread.State state = thread.getState();
        System.out.println(state);//NEW

        thread.start();//启动
        state = thread.getState();
        System.out.println(state);//Run

        while(state!=Thread.State.TERMINATED){//不终止，就一直输出状态
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            state = thread.getState();
            System.out.println(state);
        }
    }
}
```

### 6. 线程的优先级

优先级从1~10，越高越高概率先执行

先设置优先级再启动

 .getPriority() 获取当前线程优先级

 .setPriority() 设置当前线程优先级

### 7. 守护线程 daemon

线程分为用户线程和守护线程

虚拟机必须确保用户线程执行完毕，例如main

不用等待守护线程执行完毕，例如后才记录操作日志，监控内存，垃圾回收

### 8. 线程同步

并发：多个线程操作同一个资源

多个线程同时访问一个对象，需要进入这个对象的等待池子形成队列，等待前面进程使用完毕。

线程同步=队列+锁

锁机制：synchronized

```java
package com.thread.syn;

public class UnsafeBank {
    public static void main(String[] args) {
        Account account = new Account(100,"结婚基金");

        Drawing you = new Drawing(account,50, "你");
        Drawing gf = new Drawing(account,100, "GF");

        you.start();
        gf.start();
    }
}
//账户
class Account{
    public int money;//余额
    String name;//姓名

    public Account(int money, String name) {
        this.money = money;
        this.name = name;
    }
}

//银行：模拟取款
class Drawing extends Thread{
    Account account;//账户
    //取了多少钱
    int drawingMoney;
    //现在还有多少钱
    int nowMoney;

    public Drawing(Account account, int drawingMoney, String name) {
        super(name);
        this.account = account;
        this.drawingMoney = drawingMoney;
    }

    // 取钱
    // synchronized 默认锁的是this
    @Override
    public void run() {
        //锁的对象是变化的量
        synchronized (account){
            if (account.money-drawingMoney<0){
                System.out.println(Thread.currentThread().getName()+"钱不够取不了");
                return;
            }

            //放大问题的发生性
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            account.money-=drawingMoney;
            nowMoney+=drawingMoney;

            //Thread.currentThread().getName() = this.getName()
            System.out.println(account.name+"余额为"+account.money);
            System.out.println(this.getName()+"手里的钱"+this.nowMoney);
        }
    }
}
```

```java
package com.thread.syn;

//不安全的买票
//线程不安全，有负数
public class UnsafeBuyTicker {
    public static void main(String[] args) {
        BuyTicket buyTicket = new BuyTicket();
        new Thread(buyTicket,"小红").start();
        new Thread(buyTicket,"小明").start();
        new Thread(buyTicket,"老师").start();
    }
}


class BuyTicket implements Runnable{

    //票
    private int ticketNums = 10;
    boolean flag = true;//外部停止方式

    @Override
    public void run() {
        //买票
        while(flag){
            try {
                buy();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    //synchronized 同步方法，锁的是this
    private synchronized void buy() throws InterruptedException {
        //判断是否邮票
        if (ticketNums<=0) {
            flag=false;
            return;
        }
        //模拟延时
        Thread.sleep(3000);
        //买票
        System.out.println(Thread.currentThread().getName()+"买到了"+ticketNums--);
    }
}
```

```java
package com.thread.syn;

import java.util.ArrayList;
import java.util.List;

//不安全的集合
public class UnsafeList {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        //不同的线程操作list同一个位置，导致线程不安全
        for (int i = 0; i < 10000; i++) {
            new Thread(()->{
                synchronized (list){
                    list.add(Thread.currentThread().getName());
                }
            }).start();
        }

        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println(list.size());
    }
}

```

### 9. JUC并发安全的包

```java
package com.thread.syn;

import java.util.concurrent.CopyOnWriteArrayList;

//测试JUC安全类型的集合
public class TestJUC {
    public static void main(String[] args) {
        CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList();
        for (int i = 0; i < 10000; i++) {
            new Thread(()->{
                list.add(Thread.currentThread().getName());
            }).start();
        }
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println(list.size());
    }
}
```

### 10. 死锁

某一个程序块同时拥有两个以上对象的锁就可能会发生死锁问题

死锁产生条件：

* 互斥条件：一个资源只能被一个进程使用
* 请求与保持：一个进程因请求资源而阻塞的时候，对已经获得的保持不放
* 不剥夺条件：进程已经获得资源，在未使用完成之前不能强行剥夺
* 循环等待条件：若干进程之间行为一个首尾相接的循环等待资源关系

```java
package com.thread;

//死锁，多个线程互相抱着对方需要的资源
public class DeadLock {
    public static void main(String[] args) {
        Makeup g1 = new Makeup(0,"小红");
        Makeup g2 = new Makeup(1,"老师");

        g1.start();
        g2.start();
    }
}

//口红
class Lipstick{

}

//镜子
class Mirror{

}

class Makeup extends Thread{


    //需要的资源只有一份 用static来保存
    static Lipstick lipstick = new Lipstick();
    static Mirror mirror = new Mirror();

    int choice;//选择
    String name;//使用化妆品的人

    public Makeup(int choice, String name) {
        this.choice = choice;
        this.name = name;
    }

    @Override
    public void run() {
        //化妆
        makeup();
    }

    //需要拿到对方的资源
    private void makeup(){
        if(choice==0){
            synchronized (lipstick){
                //获得口红的锁
                System.out.println(this.name+"获得口红的锁");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            synchronized (mirror){
                //获得镜子的锁
                System.out.println(this.name+"获得镜子的锁");
            }
        }else {
            synchronized (mirror){
                //获得镜子的锁
                System.out.println(this.name+"获得镜子的锁");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            synchronized (lipstick){
                //获得口红的锁
                System.out.println(this.name+"获得口红的锁");
            }
        }
    }
}
```

### 11. Lock 锁 

JUC包下显式定义同步锁来实现同步，同步锁使用Lock对象充当

```java
package com.juc;

import java.util.concurrent.locks.ReentrantLock;

//测试Lock锁
public class TestLock {
    public static void main(String[] args) {
        TestLock2 testLock2 = new TestLock2();

        new Thread(testLock2,"小红").start();
        new Thread(testLock2,"小明").start();
        new Thread(testLock2,"老师").start();
    }
}

class TestLock2 implements Runnable{

    int ticketNums = 10;

    //定义可重复锁
    private final ReentrantLock lock = new ReentrantLock();

    @Override
    public void run() {
        while(true){
            try{
                lock.lock();//加锁
                //保证线程安全的代码
                if(ticketNums>0){
                    try {
                        Thread.sleep(2000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName()+"买到了"+ticketNums--);
                }else{
                    break;
                }
            }finally {
                //解锁
                lock.unlock();
            }
        }
    }
}
```

![1637573777(1)](C:\Users\Ethan\Desktop\java\img\1637573777(1).png)

### 12. 线程协作

生产者消费者问题：生产者和消费者共享同一个资源，并且生产者和消费者之间相互依赖，互为条件。

* 对于生产者，没有生产产品之前，要通知消费者等待，而生产了产品之后，又需要马上通知消费者消费。
* 对于消费者，在消费之后，要通知生产者继续生产产品以供消费。
* 在该问题中，仅有synchronized是不够的
  * synchronized可以阻止并发更新同一个共享资源，实现同步
  * 但是synchronized不能来实现不同的线程之间的消息传递（通信）

Java提供了几个方法解决线程之间的通信问题：均是Object类，都只能在同步方法或者同步代码块中使用。

* wait（）线程等待其他线程通知，会释放锁
* wait（long timeout）等待指定的毫秒数
* notify（）唤醒处于等待的线程
* notifyALL（）唤醒同一个对象上所有调用wait（）方法的线程，优先级高的线程先调度

#### 管程法

* 生产者：负责生产数据的模块
* 消费者：处理数据的模块
* 缓冲区：消费者不能直接使用生产者的数据，他们之间有个缓冲区
* 生产者把数据放入缓冲区，消费者从缓冲区拿数据

```java
package com.juc;

//管程法
//生产者、消费者、产品、缓冲区
public class TestPC {
    public static void main(String[] args) {
        SynContainer container=new SynContainer();
        new Productor(container).start();
        new Consumer(container).start();
    }
}


// 生产者
class Productor extends Thread{
    SynContainer container;

    public Productor(SynContainer container) {
        this.container = container;
    }

    //生产
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            container.push(new Chicken(i));
            System.out.println("生产了第"+i+"只鸡");
        }
    }
}

// 消费者
class Consumer extends Thread{
    SynContainer container;

    public Consumer(SynContainer container) {
        this.container = container;
    }

    //生产
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("消费了第"+container.pop().id+"只鸡");
        }
    }
}

// 产品
class Chicken{
    int id;//产品编号

    public Chicken(int id) {
        this.id = id;
    }
}

//缓冲区
class SynContainer{

    //容器大小
    Chicken[] chickens= new Chicken[10];
    //容器计数器
    int count = 0;

    // 生产者放入产品
    public synchronized void push(Chicken chicken){
        //容器满 则等待
        if(this.count==this.chickens.length){
            //通知消费者消费，生产者等待
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        //如果没有满，则放入产品
        chickens[count++] = chicken;

        //通知消费者消费了
        this.notifyAll();
    }

    // 消费者消费产品
    public synchronized Chicken pop(){
        if (count==0){
            //等待生产者生产
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        //可以消费
        Chicken chicken = chickens[--count];

        //通知生产者生产
        this.notifyAll();
        return chicken;
    }
}
```

#### 信号灯法

```java
package com.juc;

//信号灯法
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

    public Player(TV tv) {
        this.tv = tv;
    }

    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            if (i%2==0){
                this.tv.play("快乐大本营");
            }else {
                this.tv.play("广告");
            }
        }
    }
}

//消费者-->观众
class Watcher extends Thread{
    TV tv;

    public Watcher(TV tv) {
        this.tv = tv;
    }

    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            this.tv.watch();
        }
    }
}

//产品-->节目
class TV{
    //演员表演的时候，观众等待
    //观众观看，演员等待

    String voice;//表演的节目
    boolean flag = true;
    //表演
    public synchronized void play(String voice){
        if (!this.flag){
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("演员表演了："+voice);
        //通知观众观看
        this.notifyAll();
        this.voice = voice;
        this.flag = !this.flag;
    }

    //观看
    public synchronized void watch(){
        if (this.flag){
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("观众观看了："+voice);
        //通知演员表演观看
        this.notifyAll();
        this.flag = !this.flag;
    }
}
```

### 13. 线程池

背景：经常创建和销毁，使用量特别大的资源。比如并发情况下的线程，对性能影响很大。

思路：提前创建好多个线程，放入线程池，使用时直接获取，使用完放回池中。可以避免创建和销毁，实现重复利用。

好处：

* 提高了响应速度，减少了创建新线程的时间
* 降低了资源消耗，不用重复创建
* 便于线程管理
  * corePoolSize 核心池大小
  * maximumPoolSize：最大线程数
  * keepAliveTime：线程没有任务时最多保持多少时间后会终止

![1637649787(1)](C:\Users\Ethan\Desktop\java\img\1637649787(1).png)

```java
package com.juc;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

//测试线程池
public class TestPool {
    public static void main(String[] args) {
        //1.创建服务，创建线程池
        //newFixedThreadPool 池子数量
        ExecutorService service = Executors.newFixedThreadPool(10);

        //执行
        service.execute(new MyThread());
        service.execute(new MyThread());
        service.execute(new MyThread());
        service.execute(new MyThread());
        service.execute(new MyThread());

        //2.关闭连接
        service.shutdown();
    }
}

class MyThread implements Runnable{

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}
```



