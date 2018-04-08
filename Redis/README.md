# Redis

很多数据库只能处理一种数据结构： 
- SQL 数据库 —— 表格 
- Memcached —— 键值对数据库，键和值 都是字符串 
- 文档数据库（CouchDB、MongoDB） —— 由 JSON/BSON 组成的文档（document）   
而一旦数据库提供的数据结构不适合去做某件 事的话，程序写起来就会非常地麻 烦和不自然。
Redis 也是键值对数据库，但和 Memcached 不同的是，Redis 的值不仅可以是字符串，它还 可以其他五种数据结构中的任意一种。

> Redis 将数据储存在内存里面，读写数据的时候都不会受到硬盘 I/O 速度的限制，所以速度极快。

## redis 的应用
- 持久化功能：将储存在内存里面的数据保存到硬盘里面，保障数据安全，方便进行数据备份和恢复。
- 发布与订阅功能：将消息同时分发给多个客户端，用于构建广播系统。
- 过期键功能：为键设置一个过期时间，让它在指定的时间之后自动被删除。
- 事务功能：原子地执行多个操作，并提供乐观锁功能，保证处理数据时的安全性。
- 脚本功能：在服务器端原子地执行多个操作，完成复杂的功能，并减少客户端与服务器之间的通信往返次数。
- 复制：为指定的 Redis 服务器创建一个或多个复制品，用于提升数据安全性，并分担读请求的负载。
- Sentinel：监控 Redis 服务器的状态，并在服务器发生故障时，进行自动故障转移。
- 集群：创建分布式数据库，每个服务器分别执行一部分写操作和读操作。


## String 字符串
Redis 中最简单的数据结构，它既可以储存文字（比如 "hello world"），又可以储存数字（比如整数 10086 和浮点数 3.14），还可以储存二进制数据（比如 10010100）。

```
SET key value
将字符串键 key 的值设置为 value ，命令返回 OK 表示设置成功。 
redis> SET msg "hello world"  OK
```
```
SET key value [NX|XX]

redis> SET nx-str "this will fail" XX                       # 键不存在，指定 XX 选项导致设置失败  
(nil)  

redis> SET nx-str "this will success" NX                    # 键不存在，所以指定 NX 选项是可行的  
OK  

redis> SET nx-str "this will fail" NX                       # 键已经存在，指定 NX 选项导致设置失败  
(nil)  

redis> SET nx-str "this will success again!" XX             # 键已经存在，指定 XX 选项是可行的  
OK

```
SET 命令还支持可选的 NX 选项和 XX 选项：
- 如果给定了 NX 选项，那么命令仅在键 key 不存在的情况下，才进行设置操作；如果键 key 已经存 在，那么 SET ... NX 命令不做动作（不会覆盖旧值）。 
- 如果给定了 XX 选项，那么命令仅在键 key 已经存在的情况下，才进行设置操作；如果键 key 不存 在，那么 SET ... XX 命令不做动作（一定会覆盖旧值）。  
- 在给定 NX 选项和 XX 选项的情况下，SET 命令在设置成功时返回 OK ，设置失败时返回 nil 。

```
获取字符串的值
GET key

  redis> SET msg "hello world"  
  OK  
  redis> GET msg  
  hello world
 
```

```
仅在键不存在的情况下进行设置
SETNX key value

  redis> SETNX new-key "i am a new key!"  
  1  
  redis> SETNX new-key "another new key here!"    # 键已经存在，设置失败  
  0  
  redis> GET new-key    # 键的值没有改变  
  i am a new key!

```

```
同时设置或获取多个字符串键的值
MSET key value [key value ...] 
MGET key [key ...] 
```

```
一次设置多个不存在的键
MSETNX key value [key value ...]
```

```
设置新值并返回旧值
GETSET key new-value 
```

```
追加内容到字符串末尾
APPEND key value


 redis> SET myPhone "nokia"  
 OK
 redis> APPEND myPhone "-1110"  
 (integer) 10
 redis> GET myPhone  
 "nokia-1110"
```

```
返回值的长度
STRLEN key
```

#### 索引
字符串的索引（index）以 0 为开始，从字符串的开头向字符串的结尾依次递增，字符串第一个字符的索 引为 0 ，字符串最后一个字符的索引 为 N-1 ，其中 N 为字符串的长度。
除了（正数）索引之外，字符串 还有负数索引：负数索引以 -1 为开始，从字符串的结尾向字符串的开头 依次递减，字符串的最后一个字符的索引 为 -N ，其中 N 为字符串的长度。

```
范围设置
SETRANGE key index value
从索引 index 开始，用 value 覆写（overwrite）给定键 key 所储存的字符串值。只接受正数索引。

```
```
范围取值
GETRANGE key start end
返回键 key 储存的字符串值中，位于 start 和 end 两个索引之间的内容（闭区间，start 和 end 会被包括 在内）。和 SETRANGE 只接受正数索引不同，GETRANGE 的索引可以是正数或者负数。

```

#### 设置和获取数字
只要储存在字符串键里面的值可以被解释为 64 位整数，或者 IEEE-754 标准的 64 位浮点数， 那么用户就可以对这个字符串键执行针对数字值的命令。

```
增加或者减少数字的值
INCRBY key increment 
DECRBY key decrement 

redis> INCRBY num 100    # 键 num 不存在，命令先将 num 的值初始化为 0 ，  
(integer) 100                         # 然后再执行加 100 操作
redis> INCRBY num 25      # 将值再加上 25 
(integer) 125
redis> DECRBY num 10    # 将值减少 10 
(integer) 115
redis> DECRBY num 50    # 将值减少 50 
(integer) 65
```

```
增一和减一
INCR key 
DECR key 
```

```
浮点数的自增和自减
INCRBYFLOAT key increment
为字符串键 key 储存的值加上浮点数增量 increment ，命令返回操作执行之后，键 key 的值。
没有相应的 DECRBYFLOAT ，但可以通过给定负值来达到 DECRBYFLOAT 的效果。
redis> SET num 10 
OK 
redis> INCRBYFLOAT num 3.14 
"13.14" 
redis> INCRBYFLOAT num -2.04    # 通过传递负值来达到做减法的效果 
"11.1"
```

#### id生成器
很多网站在创建新条目的时候，都会使用 id 生成器来为条目创建唯一标识符

#### 使用 Redis 来进行缓存
我们可以使用 Redis 来缓存一些经常会被用到、或者需要耗费大量资源的内容，通过将这些内容放到 Redis 里面（也即是内存里面），程序可以以极快的速度取得 这些内容。
举个例子，对于一个网站来说，如果某个页面经常会被访问到，或者创建页面时耗费的资源比较多（比 如需要多次访问数据库、生成时间比较长，等等），那么我们可以使用 Redis 将这个页面缓存起来，减 轻网站的负担，降低网站的延迟值。




 



 
