# redis 

#### 安装
```
1.下载redis
wget http://download.redis.io/releases/redis-4.0.1.tar.gz
2.解压
tar -zxvf redis-4.0.1.tar.gz
3.编译
sudo make
sudo make install
这里make的时候可能会失败  
未安装gcc  apt-get install gcc
继续报错执行 make -tar xzf redis-4.0.1.tar.gz
4.编译完成之后在/src目录下会出现 redis-servce 文件
5.启动redis 
在/src目录下执行 ./redis-servce ../redis.conf
第一个是启动redis服务器
第二个是启动服务器所需的配置

6、默认情况，Redis不是在后台运行，我们需要把redis放在后台运行
vim /usr/local/redis/etc/redis.conf
将daemonize的值改为yes

7、让redis开机自启
vim /etc/rc.local
加入
/usr/local/redis/bin/redis-server /usr/local/redis/etc/redis-conf

8.客户端链接
/usr/local/redis/bin/redis-cli 

9.停止服务
/usr/local/redis/bin/redis-cli shutdown
或者
pkill redis-server

```


#### Redis 启动警告解决 
https://blog.csdn.net/kk185800961/article/details/53326465

#### Redis连接异常的解决办法
> 在启动的时候可能存在Could not get a resource from the pool的问题  
linux 的防火墙已关闭 ping ip也是通的,  这里需要注释掉配置文件里面的 bind 127.0.0.1(注释后,即允许其他设备访问)

> 在注释之后可能存在redis正在受保护的状态下运行，并且给了你4中解决办法 
这里可以选择第四种提供一种验证的密码.需要在配置文件里面设置 "requirepass" 的属性  设置验证密码

总结
1. 查看ip是否ping的通
2. 查看防火墙是否关闭,是否允许外界访问
3. 修改redis配置文件中的本地绑定(# bind 127.0.0.1)
4. 提供验证密码

### redis集群

redis的集群需要6台redis 分为三组 每组存的数据是一样的，每组设置主从（master slave） redis存数据通过算槽位的方式来判断放在那一台上，
算法是通过对key进行hash算法。一共16638 槽位 平分到每组。

## redis的对象的两种类型的数据结构实现：
1.为了进行性能优化而特制的编码数据结构，已CPU换内存的方式来节约内存。
编码数据结构主要在对象包含的值数量比较少，或者值的体积比较小时使用
2.普通数据结构： 
 




