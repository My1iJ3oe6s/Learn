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
> 提供一个存储缓存的容器，该容器实现了存放（Put）和读取（Get）缓存的接口供外部调用。 缓存通常以<key,value>的形式存在，通过key来从缓存中获取value。

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
