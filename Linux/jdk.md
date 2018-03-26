# 安装jdk

## 查询本地是否存在jdk
```
先检查当前系统中是否存在默认的jdk。
              rpm -qa|grep gcj
删除查找的列出相关文件
              rpm -e –nodeps + 文件名
如果没有找到，rpm -qa|grep gcj命令运行后若无文件列出则不必理会。
```

## 设置root的密码
sudo root

## 安装 lrzsz （用来上传文件）
sudo -apt-get install lrzsz
通过 rz 上传 sz下载

##  在usr目录下建立java安装目录
cd /usr
mkdir java

## 将jdk-8u60-linux-x64.tar.gz拷贝到java目录下
cp /mnt/hgfs/linux/jdk-8u60-linux-x64.tar.gz /usr/java/

## 解压jdk到当前目录
tar -zxvf jdk-8u60-linux-x64.tar.gz

## 解压完之后，Java目录中会出现一个jdk1.8.0_60的目录，这就解压完成了。之后配置一下环境变量。 
编辑/etc/下的profile文件，配置环境变量
```
vi /etc/profile                  进入profile文件的编辑模式

 在最后边追加一下内容(**配置的时候一定要根据自己的目录情况而定哦！**)

export JAVA_HOME=/usr/local/java/jdk1.8.0_25  
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH
 
 ```
 
 ## 重新执行文件/etc/profile
 source /etc/profile
 ```
 将配置内容(其实是命令脚本)写到/etc/profile中目的就是让任意用户登录后，都会自动执行该脚本，
 这样这些登录的用户就可以直接使用jdk了。如果因为某个原因该脚本没有自动执行，自己手动执行以下
 就可以了（也就是执行命令source /etc/profile）
 ```
 
 ## 查看java的版本
 java -version


## 重启linux 
sudo reboot

