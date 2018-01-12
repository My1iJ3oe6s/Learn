# Guava Collection

### 1.ImmutableSet(不可变集合)

##### 创建方法
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
##### 所有不可变集合都有asList()方法,方便你用列表形式来读取集合元素

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


### 3.Multiset
>可以多次添加相同的元素,相当于Map<Object, Integer>,键为元素,值为该元素的个数
```
Multiset<String> cargoNames = HashMultiset.create();
		
		List<String> list = Lists.newArrayList("a", "b", "c", "d", "a", "a", "c");
		list.forEach(c -> {
			cargoNames.add(c);
		});
		
		System.out.println(cargoNames.count("a"));  // 获取到a的个数:3
		System.out.println(cargoNames.contains("a"));  //是否包含:true
		System.out.println(cargoNames.contains("k"));  //false
		cargoNames.setCount("d", 10);  //设置元素的个数
		System.out.println(cargoNames.count("d"));// 10
		System.out.println(cargoNames.size());  //元素的个数 包括重复的 : 16
```

|方法|描述|
|count(E)|给定元素在Multiset中的计数|
|elementSet()|Multiset中不重复元素的集合，类型为Set<E>|
|entrySet()	|和Map的entrySet类似，返回Set<Multiset.Entry<E>>，其中包含的Entry支持getElement()和getCount()方法
|add(E, int)|	增加给定元素在Multiset中的计数|
|remove(E, int)	|减少给定元素在Multiset中的计数|
|setCount(E, int)|	设置给定元素在Multiset中的计数，不可以为负数|
|size()	|返回集合元素的总个数（包括重复的元素）|
