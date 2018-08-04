

[TOC]

CountDownLatch、CyclicBarrier和Semaphore工具类提供并发控制的手段，而Exchanger工具类提供了在线程间交换数据的手段。

# 1、闭锁：CountDownLatch

## 1.1 使用场景

如果有多条线程，其中一条线程需要等待**其他所有线程**运行完之后才能运行，这样就可以使用闭锁。



> 当然也可以使用`join()`，用于让 当前线程 等待 join线程 执行结束，其**实现原理**：
>
> 当前线程 不停地检查 join线程 是否存活，
>
> * 如果  join线程 存活，则让 当前线程 永远等待，即调用 `wait(0)`。
> * 如果 join线程 终止，当前线程 的 `this.notifyAll()`方法会被调用（JVM中实现的）。



JDK1.5 之后就提供闭锁，它不但可以实现 `join()`的功能，还有额外的功能。

## 1.2 代码实现

**使用方法**：

- 先创建闭锁，设定闭锁数量为N，`CountDownLatch latch = new CountDownLatch(N);`
- 每调用一次`latch.countDown()`，N就会减1
- 调用`latch.await()`的线程会被阻塞，知道N变成0。
- 当某个线程处理速度慢，我们不能让主线程一直等待，可以使用指定时间的`await(long,TimeUnit)`，这个方法等待特定时间后，就不会阻塞当前线程。

```java
/**
 * 并发工具类——闭锁的使用
 */
public class CountDownLatchTest {
    public static void main(String[] args) {
        //初始化闭锁，并设置资源个数
        CountDownLatch latch = new CountDownLatch(2);

        Thread t1 = new Thread(new Runnable() {
            @Override
            public void run() {
                //加载资源1的代码,假设需要5s
                TimeUnit.SECONDS.sleep(5);
                System.out.println("地图资源1加载完毕！！！");
                //资源1加载完毕之后，闭锁 -1
                latch.countDown();
            }
        });

        Thread t2 = new Thread(new Runnable() {
            @Override
            public void run() {
                //加载资源2的代码,假设需要7s
                TimeUnit.SECONDS.sleep(7);
                System.out.println("地图资源2加载完毕！！！");
                //资源1加载完毕之后，闭锁 -1
                latch.countDown();
            }
        });

        Thread t3 = new Thread(new Runnable() {
            @Override
            public void run() {
                //t3线程必须等待所有资源加载完毕之后才能执行
                latch.await();
                //当闭锁数量为0时，线程才能从await()返回，执行接下来的任务
                System.out.println("地图资源已全部加载完毕！！！");
            }
        });
        
        t1.start();
        t2.start();
        t3.start();
    }
}
```

# 2、同步屏障：CyclicBarrier

## 2.1 使用场景

让一组线程到达一个屏障时被阻塞，直到最后一个线程到达屏障时，屏障才会开门，此时所有被屏障阻塞的线程才会继续运行。此外，屏障打开的同时还可以指定先执行一个额外的任务。就是一个BSP（大同步模型）的实现。

## 2.2 代码实现

使用方法：

* 创建同步屏障对象，指定 等待线程个数 和 打开屏障时优先执行的任务。
* 对线程调用 `barrier.await()`，线程会被阻塞，直到屏障被打开，这些线程执行顺序由系统随机调度。

```java
//创建同步屏障对象，设定 需要等待的线程个数 和 打开屏障时需要先执行的任务
        CyclicBarrier barrier = new CyclicBarrier(3, new Runnable() {
            @Override
            public void run() {
                //当所有线程准备完毕之后，优先执行此任务
            }
        });
        //启动3条线程
        for(int i = 0; i < 3; i++) {
            new Thread(new Runnable() {
                @Override
                public void run() {
                    //等待，每执行一次barrier.wait()，同步屏障数量-1，直到为0，打开屏障
                    barrier.await();
                    //任务
                    任务代码...
                }
            }).start();
        }
    }
```

## 2.3 闭锁 与 同步屏障 的区别

* 闭锁 的计数器只能使用一次。
* 同步屏障的计数器 可以调用`reset()`重置，能处理更为复杂的业务，如：计算错误，可以重置计数器，并让线程重新计算。
* 同步屏障还提供一些其他方法，如：`getNumberWaiting()`获得阻塞的线程数量，`isBroken()`了解阻塞的线程是否被中断。
* 闭锁只会阻塞一条线程，目的是为了让该条任务线程满足条件后执行。
* 而同步屏障会阻塞所有线程，目的是为了让所有线程同时执行（实际上并不会同时执行，而是尽量把线程启动的时间间隔降为最少）。

# 3、信号量：Semaphore

## 3.1 使用场景

若有m个资源，但是有n条线程(n > m)，在某些场景下，同一时刻只允许m条线程访问资源，此时可以用 信号量 Semaphore来控制访问资源的线程数量。一般用于 **公共资源有限的场景**，如做 **流量控制**等。

## 3.2 代码实现

使用方法：

* 创建信号量对象，并指定 资源的数量。
* 在线程中调用 `semaphore.acquire()` 获取资源，若资源被用光，则会被阻塞，直到有线程归还资源。
* 调用 `semaphore.release()` 可以释放资源。

```java
/**
 * 同步工具使用——信号量
 */
public class SemaphoreTest {
    public static void main(String[] args) throws InterruptedException {
        Semaphore semaphore = new Semaphore(3);

        long start = System.currentTimeMillis();
        //开启num条线程
        int num = 9;
        Thread[] threads = new Thread[num];
        for(int i = 0; i < num; i++) {
            threads[i] = new Thread(new Runnable() {
                @Override
                public void run() {
                    //获取资源，若资源被用光，则会被阻塞，直到有线程归还资源
                    semaphore.acquire();
                    //任务代码，假设需要执行5s
                    TimeUnit.SECONDS.sleep(5);
                    //释放资源
                    semaphore.release();
                }
            });
            threads[i].start();
        }
        //是主线程在num条线程执行完毕之后，在输出运行时间
        for(int i = 0; i < num; i++) {
            threads[i].join();
        }
        long end = System.currentTimeMillis();
        //观察输出时间
        System.out.println((end - start)/1000.0 + "s");
    }
}
```

可以观察到运行时间大约为：开启的线程数量 / 资源数量 * 每个线程运行的时间，15s左右。

# 4、线程间交换者：Exchanger

## 4.1 使用场景

是用于 线程间协作 的工具类，提供一个同步点，只有两个线程都到达同步点后，两个线程才可以交换彼此的数据。可以用在 遗传算法 、 校对工作等场景，如 AB两人录入数据，系统会在两人录完之后进行比对，看是否一致。

## 4.2 代码实现

使用方法：

* 创建Exchanger对象，并在泛型中指定传输的数据类型，如String
* 在2个线程间分别调用`exchange(str)`，
  * 如果两个线程有一个没有执行该方法，则会一直等待；
  * 一旦两个线程都执行了该方法，线程会从该方法返回，并从方法得到交换的数据。

下面的代码模拟了 **银行流水校对工作** 的场景：

```java
/**
 * 线程间交换数据——Exchanger
 */
public class ExchangerTest {
    public static void main(String[] args) {
        //创建Exchanger，并指定交换的数据类型是String
        Exchanger<String> exchanger = new Exchanger<>();
        //利用Executors工具类生成一个线程池
        ExecutorService pool = Executors.newFixedThreadPool(2);

        pool.execute(new Runnable() {
            @Override
            public void run() {
                //将 A 记录的流水 交换给 另一个线程，并当2个线程都执行了exchange(str)方法后，会将str传递给对方
                String B = exchanger.exchange("银行流水A");
                //打印从另一个线程B获得的数据
                System.out.println("B的记录是:" + B);
            }
        });

        pool.execute(new Runnable() {
            @Override
            public void run() {
                String B = "银行流水B";
                //获得另外一个线程传递过来的数据，并把自己的
                String A = exchanger.exchange(B);
                //打印从另一个线程B获得的数据
                System.out.println("A的记录是:" + A);
                //若A和B记录的数据一致，则提交，否则失败
                if(A.equals(B)) {
                    System.out.println("匹配，提交数据库！！！");
                } else {
                    System.out.println("匹配失败，请再次核对！！！");
                }
            }
        });
        //关闭线程池
        pool.shutdown();
    }
}
```


