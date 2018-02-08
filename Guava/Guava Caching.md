# Guava Caching

### 前言
```
在多线程高并发场景中往往是离不开cache的，需要根据不同的应用场景来需要选择不同的cache，比如分布式缓存如redis、memcached，还有本地（进程内）缓存如ehcache、GuavaCache。之前用spring cache的时候集成的是ehcache，但接触到GuavaCache之后，被它的简单、强大、及轻量级所吸引。它不需要配置文件，使用起来和ConcurrentHashMap一样简单，而且能覆盖绝大多数使用cache的场景需求！

GuavaCache是google开源java类库Guava的其中一个模块，在maven工程下使用可在pom文件加入如下依赖：
```

```
<dependency>  
    <groupId>com.google.guava</groupId>  
    <artifactId>guava</artifactId>  
    <version>19.0</version>  
</dependency>  
```

### Cache的作用
```
提供一个存储缓存的容器，该容器实现了存放（Put）和读取（Get）缓存的接口供外部调用。 缓存通常以<key,value>的形式存在，通过key来从缓存中
获取value。
```
### Cache的api
```
@Beta
@GwtCompatible
public interface Cache<K, V> {

  //获取特定key的value
  @Nullable
  V getIfPresent(Object key);

  //获取某个key值的value,没找到的情况下进行回调,返回值不能为null
  V get(K key, Callable<? extends V> valueLoader) throws ExecutionException;

  //获取List<key>的值
  ImmutableMap<K, V> getAllPresent(Iterable<?> keys);

  //保存
  void put(K key, V value);

  //批量保存
  void putAll(Map<? extends K,? extends V> m);

  //删除某个值
  void invalidate(Object key);

  //指定批量删除
  void invalidateAll(Iterable<?> keys);

  //清除所有
  void invalidateAll();

  //数量
  long size();

  //返回状态(包括命中失败的次数...)
  CacheStats stats();

  //转换为map(会更新缓存的时间,影响到定时删除的策略)
  ConcurrentMap<K, V> asMap();

  //清除
  void cleanUp();
}

```

### 创建案例
```
public static Cache<String, Map<String, String>> cache = CacheBuilder.newBuilder()
			.expireAfterAccess(1, TimeUnit.HOURS).maximumSize(10).build();

Map<String, String> m = cache.get(key, new Callable<Map<String, String>>() {
				@Override
				public Map<String, String> call() {
					Map<String, String> map = Maps.newHashMap();
                    			map.put("key", "value");
					return map;
				}
			});

```

### 清除缓存的策略

> 任何Cache的容量都是有限的，而缓存清除策略就是决定数据在什么时候应该被清理掉。GuavaCache提了以下几种清除策略：

###### 基于存活时间的清除（Timed Eviction）

这应该是最常用的清除策略，在构建Cache实例的时候，CacheBuilder提供两种基于存活时间的构建方法：
（1）expireAfterAccess(long, TimeUnit)：缓存项在创建后，在给定时间内没有被读/写访问，则清除。
（2）expireAfterWrite(long, TimeUnit)：缓存项在创建后，在给定时间内没有被写访问（创建或覆盖），则清除。
expireAfterWrite()方法有些类似于redis中的expire命令，但显然它只能设置所有缓存都具有相同的存活时间。若遇到一些缓存数据的存活时间为1分钟，一些为
5分钟，那只能构建两个Cache实例了。

###### 基于容量的清除（size-based eviction）

在构建Cache实例的时候，通过CacheBuilder.maximumSize(long)方法可以设置Cache的最大容量数，当缓存数量达到或接近该最大值时，Cache将清除掉那些最近最
少使用的缓存。以上是这种方式是以缓存的“数量”作为容量的计算方式，还有另外一种基于“权重”的计算方式。比如每一项缓存所占据的内存空间大小都不一样，可以
看作它们有不同的“权重”（weights）。你可以使用CacheBuilder.weigher(Weigher)指定一个权重函数，并且用CacheBuilder.maximumWeight(long)指定最大总重。

###### 显式清除

任何时候，你都可以显式地清除缓存项，而不是等到它被回收，Cache接口提供了如下API：
（1）个别清除：Cache.invalidate(key)
（2）批量清除：Cache.invalidateAll(keys)
（3）清除所有缓存项：Cache.invalidateAll()

### 清除什么时候发生？
```
也许这个问题有点奇怪，如果设置的存活时间为一分钟，难道不是一分钟后这个key就会立即清除掉吗？我们来分析一下如果要实现这个功能，那Cache中就必须存在线
程来进行周期性地检查、清除等工作，很多cache如redis、ehcache都是这样实现的。但在GuavaCache中，并不存在任何线程！它实现机制是在写操作时顺带做少量的
维护工作（如清除），偶尔在读操作时做（如果写操作实在太少的话），也就是说在使用的是调用线程，参考如下示例：
```
###### 例子
```
public static Cache<Integer, String> cacheTest = CacheBuilder.newBuilder()
			.expireAfterWrite(5, TimeUnit.SECONDS).maximumSize(10).build();
	
	public static void main(String[] args) throws InterruptedException {
		cacheTest.put(1, "a");
		System.out.println(cacheTest.getIfPresent(1));
		Thread.sleep(1000);
		cacheTest.put(2, "b");
		Thread.sleep(2000);
		cacheTest.put(3, "c");
		Thread.sleep(2000);
		System.out.println(cacheTest.size());
		System.out.println(cacheTest.getIfPresent(1));
		System.out.println(cacheTest.size());
	}
```
###### 结果
```
a
3
null
2
null
//可以看出key=1的值在超时的时候没有被删除,而当调用get()的时候已经取不到值,应该在此时删除
```

###### 例子
```
 public static void main(String[] args) throws ExecutionException, InterruptedException {
	        //缓存接口这里是LoadingCache，LoadingCache在缓存项不存在时可以自动加载缓存
	        LoadingCache<Integer,String> studentCache
	                //CacheBuilder的构造函数是私有的，只能通过其静态方法newBuilder()来获得CacheBuilder的实例
	                = CacheBuilder.newBuilder()
	                //设置并发级别为8，并发级别是指可以同时写缓存的线程数
	                .concurrencyLevel(8)
	                //设置写缓存后8秒钟过期
	                .expireAfterWrite(8, TimeUnit.SECONDS)
	                //设置缓存容器的初始容量为10
	                .initialCapacity(10)
	                //设置缓存最大容量为100，超过100之后就会按照LRU最近虽少使用算法来移除缓存项
	                .maximumSize(100)
	                //设置要统计缓存的命中率
	                .recordStats()
	                //设置缓存的移除通知
	                //======================================
	                //build方法中可以指定CacheLoader，在缓存不存在时通过CacheLoader的实现自动加载缓存
	                .build(
	                        new CacheLoader<Integer, String>() {
	                            @Override
	                            public String load(Integer key) throws Exception {
	                                System.out.println("load student " + key);
	                               
	                                return key.toString();
	                            }
	                        }
	                );

	        for (int i=0;i<20;i++) {
	            //从缓存中得到数据，由于我们没有设置过缓存，所以需要通过CacheLoader加载缓存数据
	            String student = studentCache.get(1);
	            System.out.println(student);
	            //休眠1秒
	            TimeUnit.SECONDS.sleep(1);
	        }

	        System.out.println("cache stats:");
	        //最后打印缓存的命中率等 情况
	        System.out.println(studentCache.stats().toString());
	    }
```

### 查看缓存移除的监听
[缓存移除监听](http://blog.csdn.net/aitangyong/article/details/53127605)

