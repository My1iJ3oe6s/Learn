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

## 缓存雪崩
缓存雪崩可能是因为数据未加载到缓存中，或者缓存同一时间大面积的失效，从而导致所有请求都去查数据库，导致数据库CPU和内存负载过高，甚至宕机。
解决思路：
1，采用加锁计数，或者使用合理的队列数量来避免缓存失效时对数据库造成太大的压力。这种办法虽然能缓解数据库的压力，但是同时又降低了系统的吞吐量。
2，分析用户行为，尽量让失效时间点均匀分布。避免缓存雪崩的出现。
3，如果是因为某台缓存服务器宕机，可以考虑做主备，比如：redis主备，但是双缓存涉及到更新事务的问题，update可能读到脏数据，需要好好解决。

## 缓存穿透
缓存穿透是指用户查询数据，在数据库没有，自然在缓存中也不会有。这样就导致用户查询的时候，在缓存中找不到，每次都要去数据库中查询。
解决思路：
1，如果查询数据库也为空，直接设置一个默认值存放到缓存，这样第二次到缓冲中获取就有值了，而不会继续访问数据库，这种办法最简单粗暴。
2，根据缓存数据Key的规则。例如我们公司是做机顶盒的，缓存数据以Mac为Key，Mac是有规则，如果不符合规则就过滤掉，这样可以过滤一部分查询。在做缓存规划的时候，Key有一定规则的话，可以采取这种办法。这种办法只能缓解一部分的压力，过滤和系统无关的查询，但是无法根治。
3，采用布隆过滤器，将所有可能存在的数据哈希到一个足够大的BitSet中，不存在的数据将会被拦截掉，从而避免了对底层存储系统的查询压力。关于布隆过滤器，详情查看：基于BitSet的布隆过滤器(Bloom Filter) 
大并发的缓存穿透会导致缓存雪崩。



## String 字符串

## List 散列


 
