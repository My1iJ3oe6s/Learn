## Three Method Create Thread

### Java多线程实例 3种实现方法

#### Java中的多线程有三种实现方式：
1. 继承Thread类，重写run方法。Thread本质上也是一个实现了Runnable的实例，他代表一个线程的实例，并且启动线程的唯一方法就是通过Thread类的start方法。
2. 实现Runnable接口，并实现该接口的run()方法.创建一个Thread对象，用实现的Runnable接口的对象作为参数实例化Thread对象，调用此对象的start方法。
3. 实现Callable接口，重写call方法。Callable接口与Runnable接口的功能类似，但提供了比Runnable更强大的功能。有以下三点
- .Callable可以在人物结束后提供一个返回值，Runnable没有提供这个功能。
- .Callable中的call方法可以抛出异常，而Runnable的run方法不能抛出异常。
- .运行Callable可以拿到一个Future对象，表示异步计算的结果，提供了检查计算是否完成的方法。

》 需要注意的是，无论用那种方式实现了多线程，调用start方法并不意味着立即执行多线程代码，而是使得线程变为可运行状态。

#### run start的区别
```
start方法是启动一个线程，而线程中的run方法来完成实际的操作。
如果开发人员直接调用run方法，那么就会将这个方法当作一个普通函数来调用，并没有多开辟线程，开发人员如果希望多线程异步执行，则需要调用start方法。
```

#### sleep wait的区别
```
1.两者处理的机制不同，sleep方法主要是，让线程暂停执行一段时间，时间一到自动恢复，并不会释放所占用的锁，当调用wait方法以后，他会释放所占用的对象锁，等待其他线程调用notify方法才会再次醒来。
2.sleep是Threa的静态方法，是用来控制线程自身流程的，而wait是object的方法，用于进行线程通信。
3.两者使用的区域不同。sleep可以在任何地方使用，wait必须放在同步控制方法，或者语句块中执行。
```
