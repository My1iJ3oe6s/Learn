# Nginx

## 查询nginx的位置
whereis Nginx

## 判断Nignx的服务是否启动
ps aux | grep nginx

## 判断80端口被谁占用
netstat -tulnp | grep :80

## 关闭
kill -9 nginx

./nginx -s quit  等待niginx的工作做完才关闭

./nginx -s stop  直接关闭


## 配置Nginx的反向代理
反向代理： nginx等待客户的请求
正向代理： nginx主动发出去的请求

## 在http里面配置代理
```
upstream tomcat1{
  server 192.168.239.128：8080;
}
```
```
upstream tomcat1{
  server 192.168.239.131：8080 weight=10;
  server 192.168.239.132：8080 weight=10
}
```

## 在server里面配置代理拦截
proxy_pass http：//tomcat1；

配置在location里面

## 测试nginx的三种能力
转发
故障恢复能力
恢复添加

## nginx的备机方案（备机  高可用HA)



## eepalived VS zookeeper
两者都可以做高可用HA，那么有什么区别呢？
1. 主被动的角度考虑
    我们知道，nginx server通常和keepalived进行结合，那么keepalived是怎么知道nginx是否存活呢?是nginx主动向keepalived汇报信息？不是的。keepalived是主动向nginx发送请求，如果有响应，那么则nginx可用。
    对于zookeeper而言，HDFS，HBase，Yarn基于zookeeper做高可用，这里的zookeeper就是被动的，也就是说HDFS，HBase，Yarn主动向zookeeper中写数据。
2. 负载的角度来考虑
    keepalived可以帮助我们做到主从，主从的划分是通过配置文件（主从的priority之差>50）指定的，如果主没有挂掉，那么大量的请求通过主然后负载到后端的nginx，而从如果想要起作用只有等到主挂掉。
    而利用zookeeper做HA，zookeeper中可以说是“人人平等”，客户端无论访问follower，还是observer，异或是leader，都能给我们返回相应的结果，可以很好的实现了负载均衡，这也可以说是zookeeper的一个优点。
3. 存储数据的角度
    keepalived不可以存储数据，假设keepalived的主现在有50个连接，如果没有外部数据库存储这些连接的信息，主挂了的话，连接信息也就丢了，所以使用keepalived需要一个外部的数据库，但是如果主挂了的同时数据库也挂了，那么就over了，信息就会丢失，或者从起来后，连不上数据库，那么之前的连接信息也会丢失。
    zookeeper可以存储数据，zookeeper中可以创建一个zNode，里面存放数据，zookeeper可以做到一个分布式数据的一致性，zookeeper中每个节点的视图是一致的，数据本身可以做到最终一致性，也就是说其中一个server挂了，其他的server还有存的数据，那么这样的话就不需要额外的数据库，zookeeper本身就可以存储一定量的信息。这也可以说是zookeeper的另一个优点。
4. 业务的角度
    keepalived可以说比较简单，只需要简单的配置一下就可以了，使用keepalived的场景：如果我们只需要简单的知道当前的业务中哪个是主，哪个是从，那么可以选用keepalived。
    如果除了高可用以外，比如kafka，storm等还要想zookeeper中写一些数据，这时候就需要zookeeper。








