## AIO / NIO

## 阻塞和非阻塞
阻塞和非阻塞时进程在访问数据的时候，数据内是否准备就绪的一种处理方式当数据没有准备好的时候
阻塞：往往需要等待缓冲区中的数据准备好之后才处理其他的事情，否则一直等待在哪里
非阻塞：当我们的进程访问我们的数据缓冲区的时候，数据没有准备好的时候直接放回 不要等待，数据有的时候可以直接返回

## 同步和异步
同步和异步都是基于了应用程序和操作系统处理IO时间锁采用的方式
同步：应用程序要参与IO读写的操作，在处理IO事件的时候必须阻塞在某个方法上面等待我们的IO事件完成
异步： 所有的IO读写交给了操作系统

java IO模型
BIO: 阻塞IO
NIO: 实现IO事件的轮询方式 同步非阻塞的模式
AIO: 

## NIO学习资料
### 网络编程的定义：
	https://blog.csdn.net/canot/article/details/50587372
	https://blog.csdn.net/canot/article/details/50588133
	https://blog.csdn.net/a724888/article/details/75048252

### BIO:
	https://blog.csdn.net/a724888/article/details/70313748

### 5种Io模型
	https://blog.csdn.net/canot/article/details/51283430
	https://blog.csdn.net/qq924862077/article/details/81026721
	https://blog.csdn.net/a724888/article/details/74926209
	https://blog.csdn.net/qq924862077/article/details/53014784

### nio：
	https://blog.csdn.net/canot/article/details/51283430
	总结
	https://blog.csdn.net/jianjun200607/article/details/50190177
	https://blog.csdn.net/a724888/article/details/74779324
	
	
	buffer的api
		https://blog.csdn.net/abc_key/article/details/29909375
		https://blog.csdn.net/abc_key/article/details/30751263
		https://blog.csdn.net/jianjun200607/article/details/50286767
		https://blog.csdn.net/qq_28051453/article/details/71603481	
		https://blog.csdn.net/qq_30118563/article/details/80363773
		
    分散与聚集
			https://blog.csdn.net/jianjun200607/article/details/50370070
			https://blog.csdn.net/qq_28051453/article/details/71630015

	channel
		https://blog.csdn.net/abc_key/article/details/31856233
		https://blog.csdn.net/abc_key/article/details/32736041	
		https://blog.csdn.net/qq_28051453/article/details/71616582
		
		通道到通道的操作
			https://blog.csdn.net/jianjun200607/article/details/50401437
		  socketchannel
			  https://blog.csdn.net/jianjun200607/article/details/50598574
		  serversocketchannel
			  https://blog.csdn.net/jianjun200607/article/details/50777570
      数据报通道
			  https://blog.csdn.net/jianjun200607/article/details/50950999

	内存映射文件
		https://blog.csdn.net/abc_key/article/details/32435789
		https://blog.csdn.net/jianjun200607/article/details/50532679
		https://blog.csdn.net/a724888/article/details/74779324

	select
		https://blog.csdn.net/abc_key/article/details/33415261
		https://blog.csdn.net/canot/article/details/51372651
		https://blog.csdn.net/hao707822882/article/details/39548827
		https://blog.csdn.net/jianjun200607/article/details/50454015
		
		Linux epoll实现原理详解
			https://blog.csdn.net/a724888/article/details/73201103
		
	Pipe（管道）
		https://blog.csdn.net/jianjun200607/article/details/50993017
	
	NIO DEMO:
		https://blog.csdn.net/canot/article/details/51372651
		
### NIO与io的比较
	https://blog.csdn.net/jianjun200607/article/details/50993108
	https://blog.csdn.net/qq_28051453/article/details/71600487
		
### AIO：
	https://blog.csdn.net/canot/article/details/80377131
	
### 阻塞与非阻塞 异步与非异步
	https://blog.csdn.net/canot/article/details/72590556
	https://blog.csdn.net/qq924862077/article/details/81026721
	https://blog.csdn.net/a724888/article/details/75114747

### Reactor模式
	https://blog.csdn.net/qq924862077/article/details/81026740
  https://blog.csdn.net/a724888/article/details/70598648
	https://blog.csdn.net/a724888/article/details/74778221
	
### 非阻塞服务的设计
	https://blog.csdn.net/jianjun200607/article/details/50810105
	
### 网络编程
	https://blog.csdn.net/column/details/high-perf-network.html

