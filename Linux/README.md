# Linux

### 1.Linux 根目录下的dev文件夹里面放的是硬件设备的文件，如硬盘 u盘

### 2.shell指令

|pwd| 当前目录的位置|
|-|-|
|cd|切换目录|
|ls|查看当前目录下的文件 -l（详细信息） -a（查看隐藏文件）|
|cat| 查看文件|
|ctrl+l|清屏 clear|
|who| 查看当前用户|
|w| 查看当前登陆系统的用户信息|
|uname| 操作系统的信息|
|tail|查看文件的最后几行的命令 tail -1 /etc/passwd|

### 3.Linux的用户管理
|功能|指令|描述|参数描述|
|-|-|-|-|
|添加用户|useradd xxx|在/etc/passwd/   产生一个用户的账号记录|-d 创建用户时指定用户的主目录 -m 不创建客户的宿主目录即伪用户 -g用户组 -G附加组|
|||在/etc/shadow 产生一条密码的记录|-s 指定用户的登陆shell -u 指定用户的用户号 |
|删除用户|userdel 选项的用户名|删除该客户在/etc/passwd对应的用户登陆账号信息 必要的时候还要删除用户的主目录|常用的选项是-r ，它的作用是把用户的主目录一起删除|
|修改用户信息|usermod xxx||其常用的选项-c -d -m -g -G -s -u（与useradd 命令中的选项一样）|
```
例子：指定宿主目录 和登陆的shell 及查询账号密码
useradd -d /opt/test -s /bin/sh test
tail -1 /etc/passwd

指定用户的UID 2018 和起始组 root 附加组 ftp 登陆的shell /bin/sh
useradd -u 2018 -g root -G ftp -s /bin/sh 2018
```
创建用户之后 在该用户之下会出现一些隐藏的模板文件 该模板文件和/etc/skel 里面的脚本文件一样

复制用户的脚本文件
cp -fr /etc/skel/.*  /home/2018
```
创建用户的时候是没有密码的，需要设置
交互式设置用户的密码
passwd 用户名

无交互模式设置用户的密码
echo 123456 | passwd --stdin sam      -- echo表示输出一个数据   | 表示重定向  

切换用户
su - 用户名
```

