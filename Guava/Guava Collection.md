# Guava Collection

### 1.ImmutableSet(不可变集合)

#### 创建方法
- of()
``` 
public static final ImmutableSet<String> COLOR_NAMES = ImmutableSet.of(
        "red","orange","yellow","green","blue","purple"); 
```

- copyOf()
```
//复制其他集合变为一个不可变的集合
ImmutableSet<String> bar = ImmutableSet.copyOf(bars);
```

- Builder工具
```
ImmutableSet<String> of = ImmutableSet.of("7", "a", "b", "1", "c");

ImmutableSet<Object> build = ImmutableSet.builder().addAll(of).add("q").build();
```
>所有不可变集合都有asList()方法,方便你用列表形式来读取集合元素

```
ImmutableSet<Object> build = ImmutableSet.builder().addAll(of).add("q").build();

		ImmutableList<Object> asList = build.asList();
		
		Object first = asList.get(0);
		
		asList.forEach(c -> {
			System.out.println(c.toString());
		});
```


### 2.ImmutableSortedSet(有序不可变集合)
```
//该集合的排序在构建结合的时候完成
ImmutableSortedSet.of("a", "b", "c", "a", "d", "b");
//排序结果为 a,b,c,d
```
