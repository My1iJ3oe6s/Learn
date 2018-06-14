## MySQL

####安装
apt-get update
apt-get install mysql-server

####查看服务是否启动，服务启停命令及启动状态。
$ netstat -anp | grep mysql
$ service mysql start
$ service mysql stop
$ service mysql status

####查看监听端口是否正常
netstat -anpt

监听得地址如果是:::3306或者是0.0.0.0:3306，表示监听所有IP地址，这监听状态是正常。若出现127.0.0.0:3306，说明监听的本地地址，需要在mysql配置文件中将bind-address选项设置为bind-address = 0.0.0.0，重启mysql。

####解决仅本机返回的问题
whereis mysql
cd /etc/mysql
vi my.cfg  (修改 bind-address = 0.0.0.0)
