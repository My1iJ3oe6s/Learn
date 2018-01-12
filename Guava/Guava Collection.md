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
ImmutableSet<String> bar = ImmutableSet.copyOf(bars);
```
