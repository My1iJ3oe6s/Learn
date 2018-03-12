## FluentIterable 

#### FluentIterable API Test

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

