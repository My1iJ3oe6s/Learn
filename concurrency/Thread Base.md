#JAVA 多线程
 ### 1.学习的方面
 1. 线程的启动
 2. 如何使线程暂停
 3. 线程停止
 4. 线程的优先级
 5. 线程安全
 
 ### 多线程的作用
 > 可以最大程度的利用CPU的空闲时间来处理其他的任务
 
 ### api
 1. start（）
 start方法通知“线程规划器”此线程已经准备就绪，等待调用线程对象的run（）方法
 执行start方法的顺序不代表线程启动的顺序
 
 2. currentThread（） 表示当前线程

 3. isAlive（） 表示当前线程是否处于活跃的状态（该状态值线程启动且尚未终止。）
 
 4. sleep（） 正在执行的线程休眠 （this.currentThread()）
 
 5. getId() 获取线程的唯一标识
 
 6. 停止线程 interrupt（）中断线程  （ 调用interrupt方法仅仅实在当前线程中打了一个停止的标签，并不是真正的停止线程）
 
 
 