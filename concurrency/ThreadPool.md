## 线程池

#### 1. ThreadPoolExecutor

    java.uitl.concurrent.ThreadPoolExecutor类是线程池中最核心的一个类.

#### 2. ThreadPoolExecutor的构造方法

```
//在ThreadPoolExecutor类中提供了四个构造方法：

public class ThreadPoolExecutor extends AbstractExecutorService {
    .....
    public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit,
            BlockingQueue<Runnable> workQueue);
 
    public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit,
            BlockingQueue<Runnable> workQueue,ThreadFactory threadFactory);
 
    public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit,
            BlockingQueue<Runnable> workQueue,RejectedExecutionHandler handler);
 
    public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit,
        BlockingQueue<Runnable> workQueue,ThreadFactory threadFactory,RejectedExecutionHandler handler);
    ...
}
```

    从上面的代码可以得知，ThreadPoolExecutor继承了AbstractExecutorService类，并提供了四个构造器，事实上，通过观察每
    个构造器的源码具体实现，发现前面三个构造器都是调用的第四个构造器进行的初始化工作。

- corePoolSize：核心池的大小，这个参数跟后面讲述的线程池的实现原理有非常大的关系。在创建了线程池后，默认情况下，线程池中
  并没有任何线程，而是等待有任务到来才创建线程去执行任务，除非调用了prestartAllCoreThreads()或者prestartCoreThread()方法
  ，从这2个方法的名字就可以看出，是预创建线程的意思，即在没有任务到来之前就创建corePoolSize个线程或者一个线程。默认情况下，
  在创建了线程池后，线程池中的线程数为0，当有任务来之后，就会创建一个线程去执行任务，当线程池中的线程数目达到corePoolSize后
  ，就会把到达的任务放到缓存队列当中；

- maximumPoolSize：线程池最大线程数，这个参数也是一个非常重要的参数，它表示在线程池中最多能创建多少个线程；

- keepAliveTime：表示线程没有任务执行时最多保持多久时间会终止。默认情况下，只有当线程池中的线程数大于corePoolSize时
  ，keepAliveTime才会起作用，直到线程池中的线程数不大于corePoolSize，即当线程池中的线程数大于corePoolSize时，如果一个
  线程空闲的时间达到keepAliveTime，则会终止，直到线程池中的线程数不超过corePoolSize。但是如果调用了
  allowCoreThreadTimeOut(boolean)方法，在线程池中的线程数不大于corePoolSize时，keepAliveTime参数也会起作用，直到线程
  池中的线程数为0；
  
- workQueue：一个阻塞队列，用来存储等待执行的任务，这个参数的选择也很重要，会对线程池的运行过程产生重大影响，一般来说，这
  里的阻塞队列有以下几种选择：
```  
    ArrayBlockingQueue：基于数组的先进先出队列，此队列创建时必须指定大小；
    LinkedBlockingQueue：基于链表的先进先出队列，如果创建时没有指定此队列大小，则默认为Integer.MAX_VALUE；
    SynchronousQueue: 这个队列比较特殊，它不会保存提交的任务，而是将直接新建一个线程来执行新来的任务。
```

- threadFactory：线程工厂，主要用来创建线程；

- handler：表示当拒绝处理任务时的策略，有以下四种取值：
```
    ThreadPoolExecutor.AbortPolicy:丢弃任务并抛出RejectedExecutionException异常。 
    ThreadPoolExecutor.DiscardPolicy：也是丢弃任务，但是不抛出异常。 
    ThreadPoolExecutor.DiscardOldestPolicy：丢弃队列最前面的任务，然后重新尝试执行任务（重复此过程）
    ThreadPoolExecutor.CallerRunsPolicy：由调用线程处理该任务 
```
  
#### 3. ThreadPoolExecutor的主要方法
- execute()方法实际上是Executor中声明的方法，在ThreadPoolExecutor进行了具体的实现，这个方法是ThreadPoolExecutor的核心方法，通过这个方法可以向线程池提交一个任务，交由线程池去执行。

- submit()方法是在ExecutorService中声明的方法，在AbstractExecutorService就已经有了具体的实现，在ThreadPoolExecutor中并没有对其进行重写，这个方法也是用来向线程池提交任务的，但是它和execute()方法不同，它能够返回任务执行的结果，去看submit()方法的实现，会发现它实际上还是调用的execute()方法，只不过它利用了Future来获取任务执行结果（Future相关内容将在下一篇讲述）。

- shutdown()和shutdownNow()是用来关闭线程池的。

#### 4. ThreadPoolExecutor重要成员变量解析

```
    private final BlockingQueue<Runnable> workQueue;              //任务缓存队列，用来存放等待执行的任务
    
    private final ReentrantLock mainLock = new ReentrantLock();   //线程池的主要状态锁，对线程池状态（比如线程池大小
                                                              //、runState等）的改变都要使用这个锁
    private final HashSet<Worker> workers = new HashSet<Worker>();  //用来存放工作集
 
    private volatile long  keepAliveTime;    //线程存货时间   
    
    private volatile boolean allowCoreThreadTimeOut;   //是否允许为核心线程设置存活时间
    
    private volatile int   corePoolSize;     //核心池的大小（即线程池中的线程数目大于这个参数时，提交的任务会被放进任务
                                             //缓存队列）
    
    private volatile int   maximumPoolSize;   //线程池最大能容忍的线程数
 
    private volatile int   poolSize;       //线程池中当前的线程数
 
    private volatile RejectedExecutionHandler handler; //任务拒绝策略
 
    private volatile ThreadFactory threadFactory;   //线程工厂，用来创建线程
 
    private int largestPoolSize;   //用来记录线程池中曾经出现过的最大线程数
 
    private long completedTaskCount;   //用来记录已经执行完毕的任务个数
```


#### 5. 线程池的状态

在ThreadPoolExecutor中定义了一个volatile变量，另外定义了几个static final变量表示线程池的各个状态：
```
    volatile int runState;
    static final int RUNNING    = 0;    //runState表示当前线程池的状态，它是一个volatile变量用来保证线程之间的可见性；
    static final int SHUTDOWN   = 1;
    static final int STOP       = 2;
    static final int TERMINATED = 3;
```
- 当创建线程池后，初始时，线程池处于RUNNING状态；

- 如果调用了shutdown()方法，则线程池处于SHUTDOWN状态，此时线程池不能够接受新的任务，它会等待所有任务执行完毕；

- 如果调用了shutdownNow()方法，则线程池处于STOP状态，此时线程池不能接受新的任务，并且会去尝试终止正在执行的任务；

- 当线程池处于SHUTDOWN或STOP状态，并且所有工作线程已经销毁，任务缓存队列已经清空或执行结束后，线程池被设置为TERMINATED状态。

#### 6.任务拒绝策略
　   当线程池的任务缓存队列已满并且线程池中的线程数目达到maximumPoolSize，如果还有任务到来就会采取任务拒绝策略，
     通常有以下四种策略：

```
    ThreadPoolExecutor.AbortPolicy:丢弃任务并抛出RejectedExecutionException异常。
    ThreadPoolExecutor.DiscardPolicy：也是丢弃任务，但是不抛出异常。
    ThreadPoolExecutor.DiscardOldestPolicy：丢弃队列最前面的任务，然后重新尝试执行任务（重复此过程）
    ThreadPoolExecutor.CallerRunsPolicy：由调用线程处理该任务
```

#### 7.线程池的关闭
    ThreadPoolExecutor提供了两个方法，用于线程池的关闭，分别是shutdown()和shutdownNow()，其中：

- shutdown()：不会立即终止线程池，而是要等所有任务缓存队列中的任务都执行完后才终止，但再也不会接受新的任务
- shutdownNow()：立即终止线程池，并尝试打断正在执行的任务，并且清空任务缓存队列，返回尚未执行的任务

#### 8.ThreadPoolExecutor测试
```
public class MyThreadPool<V> implements Callable<V> {

	private V taskNum;

	public MyThreadPool(V taskNum) {
		super();
		this.taskNum = taskNum;
	}

	public V call() throws Exception {
		Thread.currentThread().sleep(5000);
		return taskNum;
	}

}
```

```
public class Test {

	// private final Integer a = 10 ;

	public static void main(String[] args) throws InterruptedException, ExecutionException {
		test1();
	}

	public static void test1() throws InterruptedException, ExecutionException {

		ThreadPoolExecutor executor = new ThreadPoolExecutor(5, 10, 20000, TimeUnit.MILLISECONDS,
				new ArrayBlockingQueue<Runnable>(10));

		for (int i = 0; i < 20; i++) {
			MyThreadPool<Integer> my = new MyThreadPool(i);
			Future<Integer> submit = executor.submit(my);
			list.add(submit);
			System.out.println("线程池中线程数目：" + executor.getPoolSize() + "，队列中等待执行的任务数目：" + executor.getQueue().size()
					+ "，已执行玩别的任务数目：" + executor.getCompletedTaskCount());
		}
		executor.shutdown();

	}

}
```
###### 结果
```
线程池中线程数目：1，队列中等待执行的任务数目：0，已执行玩别的任务数目：0
线程池中线程数目：2，队列中等待执行的任务数目：0，已执行玩别的任务数目：0
线程池中线程数目：3，队列中等待执行的任务数目：0，已执行玩别的任务数目：0
线程池中线程数目：4，队列中等待执行的任务数目：0，已执行玩别的任务数目：0
线程池中线程数目：5，队列中等待执行的任务数目：0，已执行玩别的任务数目：0
线程池中线程数目：5，队列中等待执行的任务数目：1，已执行玩别的任务数目：0
线程池中线程数目：5，队列中等待执行的任务数目：2，已执行玩别的任务数目：0
线程池中线程数目：5，队列中等待执行的任务数目：3，已执行玩别的任务数目：0
线程池中线程数目：5，队列中等待执行的任务数目：4，已执行玩别的任务数目：0
线程池中线程数目：5，队列中等待执行的任务数目：5，已执行玩别的任务数目：0
线程池中线程数目：5，队列中等待执行的任务数目：6，已执行玩别的任务数目：0
线程池中线程数目：5，队列中等待执行的任务数目：7，已执行玩别的任务数目：0
线程池中线程数目：5，队列中等待执行的任务数目：8，已执行玩别的任务数目：0
线程池中线程数目：5，队列中等待执行的任务数目：9，已执行玩别的任务数目：0
线程池中线程数目：5，队列中等待执行的任务数目：10，已执行玩别的任务数目：0
线程池中线程数目：6，队列中等待执行的任务数目：10，已执行玩别的任务数目：0
线程池中线程数目：7，队列中等待执行的任务数目：10，已执行玩别的任务数目：0
线程池中线程数目：8，队列中等待执行的任务数目：10，已执行玩别的任务数目：0
线程池中线程数目：9，队列中等待执行的任务数目：10，已执行玩别的任务数目：0
线程池中线程数目：10，队列中等待执行的任务数目：10，已执行玩别的任务数目：0
```
###### 结论
- 线创建核心线程的数量直到最大值, 在加进去的任务将放在队列中,当队列到达最大值时,创建新的线程,
直到到达线程的最大值,若线程超过最大值时会出现异常,任务拒绝策略将生效.若没设置会出现异常.
    
- callable返回的对象一定要将Future放在list集合中. 如果在for循环中使用get()方法,会导致该线程
执行完取到结果才会往下走.

#### 9.多线程的一般创建方式
```
final static ExecutorService exec = Executors.newCachedThreadPool();
```

