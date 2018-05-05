# MongoDB

目录
* [1.介绍](#介绍)
* [2.安装和配置](#安装和配置)
* [3.体系结构](#体系结构)    
* [4.shell操作命令](#shell操作命令)

## 介绍

  MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最
丰富，最像关系数据库的。他支持的数据结构非常松散，是类似 json 的 bjson 格式，因
此可以存储比较复杂的数据类型。 MongoDB 最大的特点是他支持的查询语言非常强大，其语
法有点类似于面向对象的查询语言，几乎可以实现类似关系数据库单表查询的绝大部分功能，
而且还支持对数据建立索引。它是一个面向集合的,模式自由的文档型数据库。

#### 1、面向集合（Collenction-Orented）
  意思是数据被分组存储在数据集中， 被称为一个集合（ Collenction)。每个集合在数据库
中都有一个唯一的标识名，并且可以包含无限数目的文档。集合的概念类似关系型数据库中的表,
不同的是它不需要定义任何模式(schema)。

#### 2、模式自由（schema-free)
  意味着对于存储在 MongoDB 数据库中的文件，我们不需要知道它的任何结构定义。提了这
么多次"无模式"或"模式自由"，它到是个什么概念呢？例如，下面两个记录可以存在于同一
个集合里面：
{"welcome" : "Beijing"}
{"age" : 25}

#### 3、文档型
  意思是我们存储的数据是键-值对的集合,键是字符串,值可以是数据类型集合里的任意类型,
包括数组和文档. 我们把这个数据格式称作 “ BSON” 即 “ Binary Serialized dOcument
Notation.”

### 特点
- 面向集合存储，易于存储对象类型的数据
- 模式自由
- 支持动态查询
- 支持完全索引，包含内部对象
- 支持查询
- 支持复制和故障恢复
- 使用高效的二进制数据存储，包括大型对象（如视频等）
- 自动处理碎片，以支持云计算层次的扩展性
- 支持 Python， PHP， Ruby， Java， C， C#， Javascript， Perl 及 C++语言的驱动程序，社区
  中也提供了对 Erlang 及.NET 等平台的驱动程序
- 文件存储格式为 BSON（一种 JSON 的扩展）
- 可通过网络访问

### 功能
- 面向集合的存储：适合存储对象及 JSON 形式的数据
- 动态查询： MongoDB 支持丰富的查询表达式。查询指令使用 JSON 形式的标记，可轻易
  查询文档中内嵌的对象及数组
- 完整的索引支持：包括文档内嵌对象及数组。 MongoDB 的查询优化器会分析查询表达
  式，并生成一个高效的查询计划
- 查询监视： MongoDB 包含一系列监视工具用于分析数据库操作的性能
- 复制及自动故障转移： MongoDB 数据库支持服务器之间的数据复制，支持主-从模式及
  服务器之间的相互复制。复制的主要目标是提供冗余及自动故障转移
- 高效的传统存储方式：支持二进制数据及大型对象（如照片或图片）
- 自动分片以支持云级别的伸缩性：自动分片功能支持水平的数据库集群，可动态添加额外的机器


## 安装和配置

1. MongoDB安装很简单，无需下载源文件，可以直接用apt-get命令进行安装。 
```
sudo apt-get install mongodb

//通过获取压缩文件安装
curl -O http://fastdl.mongodb.org/linux/mongodb-linux-i686-1.8.1.tgz
```
2. 查看当前MongoDB的版本
```
mongo -version
```
3. 启动和关闭mongodb命令如下：
```
service mongodb start
service mongodb stop
```
4. 输入以下命令查看是否启动成功：
```
pgrep mongo -l   #注意：-l是英文字母l，不是阿拉伯数字1
```
5. 卸载MongoDB
```
sudo apt-get --purge remove mongodb mongodb-clients mongodb-server
```
6.shell命令模式
输入mongo进入shell命令模式，默认连接的数据库是test数据库，在此之前一定要确保你已经启动了MongoDB，
否则会出现错误： 
```
mongo
```
7. 常用操作命令：
```
数据库相关 
show dbs:显示数据库列表 
show collections：显示当前数据库中的集合（类似关系数据库中的表table） 
show users：显示所有用户 
use yourDB：切换当前数据库至yourDB 
db.help() ：显示数据库操作命令 
db.yourCollection.help() ：显示集合操作命令，yourCollection是集合名 
```
8.查看mongo安装文件的位置
```
whereis mongo
```

 
## 体系结构
 一个运行着的 MongoDB 数据库就可以看成是一个 ` MongoDB Server`，该 Server 由实例和
数据库组成，在一般的情况下一个 MongoDB Server 机器上包含`一个实例`和`多个与之对应的数据
库`，但是在特殊情况下，如硬件投入成本有限或特殊的应用需求，也允许一个 Server 机器上可以有
`多个实例和多个数据库`.

MongoDB 中一系列物理文件（数据文件，日志文件等）的集合或与之对应的逻辑结构（集合，文档等）
被称为数据库，简单的说，就是数据库是由一系列与`磁盘有关系的物理文件`的组成。

MongoDB 的逻辑结构是一种层次结构。主要由：
文档(document)、集合(collection)、数据库(database)这三部分组成的。逻辑结构是面向用户
的，用户使用 MongoDB 开发应用程序使用的就是逻辑结构。
* MongoDB 的`文档`（ document）， 相当于关系数据库中的一行记录。
* 多个文档组成一个`集合`（ collection）， 相当于关系数据库的表。
* 多个集合（ collection）， 逻辑上组织在一起，就是`数据库`（ database）。
* 一个 MongoDB `实例`支持多个数据库（ database）。

|MongoDB|关系型数据库|
|-|-|
|数据库(database)|数据库(database)|
|集合(collection)|表(table)|
|文档(document)|行(row)|

数据存储结构:
  MongoDB 的默认数据目录是/data/db，它负责存储所有的 MongoDB 的数据文件。在 MongoDB
内部，每个数据库都包含一个.ns 文件和一些数据文件，而且这些数据文件会随着数据量的
增加而变得越来越多。所以如果系统中有一个叫做 foo 的数据库，那么构成 foo 这个数据库
的文件就会由 foo.ns， foo.0， foo.1， foo.2 等等组成



## shell操作命令
#### 1.mongod 的主要参数有：

* dbpath:
  数据文件存放路径，每个数据库会在其中创建一个子目录，用于防止同一个实例多次运
  行的 mongod.lock 也保存在此目录中。
* logpath
错误日志文件
* logappend
  错误日志采用追加模式（默认是覆写模式）
* bind_ip
  对外服务的绑定 ip，一般设置为空，及绑定在本机所有可用 ip 上，如有需要可以单独
  指定
* port
  对外服务端口。 Web 管理端口在这个 port 的基础上+1000
* fork
  以后台 Daemon 形式运行服务
* journal
  开启日志功能，通过保存操作日志来降低单机故障的恢复时间，在 1.8 版本后正式加入，
  取代在 1.7.5 版本中的 dur 参数。
* syncdelay
  系统同步刷新磁盘的时间，单位为秒，默认是 60 秒。
* directoryperdb
  每个 db 存放在单独的目录中，建议设置该参数。与 MySQL 的独立表空间类似
* maxConns
  最大连接数
* repairpath
  执行 repair 时的临时目录。在如果没有开启 journal，异常 down 机后重启，必须执行 repair
  操作
  
```
  mongod 的参数中，没有设置内存大小相关的参数，是的， MongoDB 使用 os mmap 机制来
缓存数据文件数据，自身目前不提供缓存机制。这样好处是代码简单，mmap 在数据量不超过内存
时效率很高。但是数据量超过系统可用内存后，则写入的性能可能不太稳定，容易出现大起大落，
不过在最新的 1.8 版本中，这个情况相对以前的版本已经有了一定程度的改善。
```

> mongod 支持将参数写入到一个配置文本文件中，然后通过 config 参数来引用此配置文件：
```
mongod --config /etc/mongo.cnf
```

#### 2.数据库管理命令
1. 修改数据库(登陆默认进入的是本机的test库)
```
use dbname
```
2. 插入记录
```
//插入两个对象 j 和 t
>j = {"name":"mongo"}

>t = {"x":3}

> db.things.save(j);

> db.things.save(t);    
(将对象保存到对应的集合(collection)中(things),这里的t代表的是集合中的文档(document))

> db.things.find();   //查询对应集合的数据  这里可以指定查询
{ "_id" : ObjectId("4c2209f9f3924d31102bd84a"), "name" : "mongo" }
{ "_id" : ObjectId("4c2209fef3924d31102bd84b"), "x" : 3 }
```

3. _id key_
  存储在 MongoDB 集合中的每个文档（ document）都有一个默认的主键_id，这个主键名称是
固定的，它可以是 MongoDB 支持的任何数据类型，默认是 ObjectId。在关系数据库 schema
设计中，主键大多是数值型的，比如常用的 int 和 long，并且更通常的是主键的取值由数据
库自增获得，这种主键数值的有序性有时也表明了某种逻辑。反观 MongoDB，它在设计之
初就定位于分布式存储系统，所以它原生的不支持自增主键。

4. 通过for循环添加数据
```
for( var i = 1; i < 10; i++ ) db.things.save( { x:4, j:i } ); > db.things.find();
{"name" : "mongo" , "_id" : ObjectId("497cf60751712cf7758fbdbb")}
{"x" : 3 , "_id" : ObjectId("497cf61651712cf7758fbdbc")}
{"x" : 4 , "j" : 1 , "_id" : ObjectId("497cf87151712cf7758fbdbd")}
{"x" : 4 , "j" : 2 , "_id" : ObjectId("497cf87151712cf7758fbdbe")}
{"x" : 4 , "j" : 3 , "_id" : ObjectId("497cf87151712cf7758fbdbf")}
{"x" : 4 , "j" : 4 , "_id" : ObjectId("497cf87151712cf7758fbdc0")}
{"x" : 4 , "j" : 5 , "_id" : ObjectId("497cf87151712cf7758fbdc1")}
```

5. 查询记录
```
// 打印所有的文档
> var cursor = db.things.find();        //返回一个游标对象
> while (cursor.hasNext()) printjson(cursor.next());      //通过游标判断是否存在下个值
{ "_id" : ObjectId("4c2209f9f3924d31102bd84a"), "name" : "mongo" }
{ "_id" : ObjectId("4c2209fef3924d31102bd84b"), "x" : 3 }
{ "_id" : ObjectId("4c220a42f3924d31102bd856"), "x" : 4, "j" : 1 }
{ "_id" : ObjectId("4c220a42f3924d31102bd857"), "x" : 4, "j" : 2 }
{ "_id" : ObjectId("4c220a42f3924d31102bd858"), "x" : 4, "j" : 3 }
```
  当我们使用的是 JavaScript shell, 可以用到 JS 的特性, forEach 就可以输出游标了. 下面的例
子就是使用 forEach() 来循环输出: forEach() 必须定义一个函数供每个游标元素调用.
```
> db.things.find().forEach(printjson);
{ "_id" : ObjectId("4c2209f9f3924d31102bd84a"), "name" : "mongo" }
{ "_id" : ObjectId("4c2209fef3924d31102bd84b"), "x" : 3 }
{ "_id" : ObjectId("4c220a42f3924d31102bd856"), "x" : 4, "j" : 1 }
```
在 MongoDB shell 里, 我们也可以把游标当作数组来用:
```
> var cursor = db.things.find();
> printjson(cursor[4]);
{ "_id" : ObjectId("4c220a42f3924d31102bd858"), "x" : 4, "j" : 3 }
```




