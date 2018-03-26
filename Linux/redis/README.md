# redis 

## 安装
```
1.下载redis
wget http://download.redis.io/releases/redis-4.0.1.tar.gz
2.解压
tar -zxvf redis-4.0.1.tar.gz
3.编译
make
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