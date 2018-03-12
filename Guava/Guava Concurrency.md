# Guava Concurrency

### 1.创建方式
```
final ListeningExecutorService executorService = MoreExecutors
				.listeningDecorator(Executors.newFixedThreadPool(NO_OF_THREADS));
```
