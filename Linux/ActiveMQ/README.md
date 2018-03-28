# ActiveMQ

> 安装包从官网下

## 启动
./activemq start > /tmp/activemq.log   (启动并将日志写入该文件下)

## 检查端口是否开启
netstat -an | grep 61616

## 在本地检查
talent 192.168.239.128 8161

## 关闭端口
ps -ef | grep activemq 来得到进程号然后kill掉

## 通过指定的配置文件来启动项目
默认情况下start启动的配置文件是/conf下的activeMq.xml
```
./activemq start xbean:file: 文件的相对路径 
```

##java项目启动broker（即activemq的客户端）
brokerService





