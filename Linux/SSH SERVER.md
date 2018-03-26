## SSH SERVER

> linux的网络是好的情况下无法通过Xshell连接到本地起的虚拟机是因为虚拟机没有开启SSH SERVER

#### 检查网络是否正常
```
ping www.baidu.com
```

#### 查询开启的端口
netstat -a 

#### 安装SSH server
sudo apt-get install openssh-server openssh-client 


#### 配置完成后重起： 
sudo /etc/init.d/ssh restart
