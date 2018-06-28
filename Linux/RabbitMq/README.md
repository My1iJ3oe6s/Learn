## RabbitMq
#### 由于rabbitMq需要erlang语言的支持，在安装rabbitMq之前需要安装erlang，执行命令：
 sudo apt-get install erlang-nox
#### 安装rabbitMq命令：
 sudo apt-get update
 sudo apt-get install rabbitmq-server
#### 启动、停止、重启、状态rabbitMq命令：
- 启动：sudo rabbitmq-server start
- 关闭： sudo rabbitmq-server stop
- 重启： sudo rabbitmq-server restart
- 查看状态：sudo rabbitmqctl status

#### 打开管理页面 
sudo rabbitmq-plugins enable rabbitmq_management
#### 查看安装的插件 
sudo rabbitmq-plugins list
#### 查看用户 
sudo rabbitmqctl list_users
#### 新增管理员用户 
sudo rabbitmqctl add_user admin admin 
sudo rabbitmqctl set_user_tags admin administrator 
#### 用刚设置的账户登录管理页面 
http://127.0.0.1:15672
