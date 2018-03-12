## FluentIterable 

#### 1.FluentIterable介绍
```
这次主要介绍是的是com.google.common.collect.FluentIterable.做为Java Iterable API的扩展，通过不同方式来提供功能类似于Java 8强大的”Streams” 库(in java.util.stream)。 
关键的不同点有: 
1. 一个stream通常是一次消费的。当一次任何”终端操作”(findFirst()或iterator())被调用时,它会变得无效。尽管流实现并包含所有Iterable接口的方法，实际上它并没有真正这样做。所以为了避免上面的情况，我们需要避免repeat-iterability。 
2. FluentIterable并没有提供Stream中许多功能,包括最小/最大,层次分明,减少,排序,非常强大的收集,内置支持并行流操作。 
3. FluentIterable提供了一些”Stream”中并不包含的特性。 
4. Streams提供了IntStream的变体，强烈推荐使用。 
5. Streams是标准的Java库，不需要依赖第三方库(但是不兼容Java 7以及以前版本) 
```

#### 2.FluentIterable API

#### 3.FluentIterable API Test

```
ArrayList<String> newArrayList = Lists.newArrayList();
		newArrayList.add("123");
		newArrayList.add("234");
		newArrayList.add("345");
		newArrayList.add("456");
		newArrayList.add("567");
		newArrayList.add("678");
```

- 返回拼接的字符串
```
String join = FluentIterable.from(newArrayList).transform(c -> {
			return c + "a";
		}).join(Joiner.on('#'));
```

- 判断是否包含
```
boolean anyMatch = FluentIterable.from(newArrayList).transform(c -> {
			return c + "a";
		}).anyMatch(c -> c.contains("123"));
```

- 创建新的集合
```
ImmutableList<String> list = FluentIterable.from(newArrayList).transform(c -> {
			return c + "a";
		}).toList();
```

- for循环
```
FluentIterable.from(newArrayList).forEach(System.out::print);
```

