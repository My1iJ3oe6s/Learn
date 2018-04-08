## 散列（hash）
一个散列由多个域值对（field-value pair）组成，散列的 域和值都可以是文字、整数、浮点数或者二 进制数据。
同一个散列里面的每个域必 须是独一无二、各不相同 的，而域的值则没有这一要求，换句话说，不同域的值 可以是重复的。
通过命令，用户可以对散列执行设置域值对、获取域的 值、检查域是否存在等操作，也可以 让 Redis 返回散 列包含的所有域、所有值或者所有域值对。

#### 指令
```
关联域值对
HSET key field value
在散列键 key 中关联给定的域值对 field 和 value 。 如果域 field 之前没有关联值，那么命令返回 1 ； 
如果域 field 已经有关联值，那么命令用新值覆盖旧值，并返 回 0 。

redis> HSET message "id" 10086 
(integer) 1 
redis> HSET message "sender" "peter" 
(integer) 1 
redis> HSET message "receiver" "jack" 
(integer) 1
```

```
HGET key field
返回散列键 key 中，域 field 所关联的值。如果域 field 没有关联值，那么返回 nil 。

redis> HGET message "id" 
"10086" 
redis> HGET message "sender" 
"peter" 
```

```
仅当域不存在时，关联域值对
HSETNX key field value
如果散列键 key 中，域 field 不存在（也即是，还没有与之相关联的值），那么关联给定的域值对 field 和 value 。
如果域 field 已经有与之相关联的值，那么命令不做动作。

redis> HSETNX message "content" "Good morning, jack!" 
(integer) 1
redis> HSETNX message "content" "Good morning, jack!" 
(integer) 0
```

```
检查域是否存在
HEXISTS key field
查看散列键 key 中，给定域 field 是否存在：存在返回 1 ，不存在返回 0 。

redis> HEXISTS message "id" 
(integer) 1 
redis> HEXISTS message "sender" 
(integer) 1 
redis> HEXISTS message "content" 
(integer) 0 
```

```
删除给定的域值对
HDEL key field [field ...]
删除散列键 key 中的一个或多个指定域，以及那些域的 值。 不存在的域将被忽略。命令返回被成功 删除的域值对数量。
redis> HDEL message "id" 
(integer) 1
redis> HDEL message "receiver" 
(integer) 1
redis> HDEL message "sender" 
(integer) 1
```

```
获取散列包含的键值对数量
HLEN key
redis> HLEN message 
(integer) 4
redis> HDEL message "date" 
(integer) 1

```

###### 批量操作
```
一次设置或获取散列中的多个域值对
HMSET key field value [field value ...] 
HMGET key field [field ...] 

redis> HMSET message "id" 10086 "sender" "peter" "receiver" "jack" 
OK
redis> HMGET message "id" "sender" "receiver" 
1) "10086" 
2) "peter" 
3) "jack"

```


```
获取散列包含的所有域、值、或者域值对
HKEYS key 
HVALS key
HGETALL key
```

```
对域的值执行自增操作
HINCRBY key field increment   为散列键 key 中， 域 field 的值加上整数增量 increment 。

HINCRBYFLOAT key field increment  为散列键 key 中， 域 field 的值加上浮点数增量 increment 。

redis> HINCRBY numbers x 100    # 域不存在，先将值初始化为 0 ，然后再执行 HINCRBY 操作 
(integer) 100 
redis> HINCRBY numbers x -50     # 传入负值，做减法 
(integer) 50 
redis> HINCRBYFLOAT numbers x 3.14    # 浮点数计算 
"53.14"

```


### 散列键和字符串键
|散列命令|字符串/数据库命令 |
|-|-|
|HSET| SET |
|HGET|GET|
|HSETNX| SETNX |
|HDEL| DEL|
|HMSET|MSET|
|HMGET|MGET|
|HINCRBY|INCRBY|
|HINCRBYFLOAT|INCRBYFLOAT|
|HEXISTS| EXISTS|






