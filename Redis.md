# 1、概述

REmote DIctionary Server(Redis) 是一个由Salvatore Sanfilippo写的key-value存储系统。

Redis是一个开源的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

它通常被称为数据结构服务器，因为值（value）可以是 字符串(String), 哈希(Hash), 列表(list), 集合(sets) 和 有序集合(sorted sets)等类型。

> Redis可以干什么？

- 内存存储，持久化
- 效率高，可以用于高速缓存
- 发布订阅系统
- 地图信息分析
- 计时器，计数器
- ......

> 特性

- 多样的数据类型
- 持久化
- 集群
- 事务
- ......

官网：https://redis.io/

中文网：http://www.redis.cn/

Github：https://github.com/antirez/redis

**官方不推荐在Window上使用。window版本需要在GitHub上下载**

# 2、Windows安装

- 下载地址：https://github.com/microsoftarchive/redis/releases

  ![image-20200504121609094](http://image.beloved.ink/Typora/image-20200504121609094.png)

- 解压下载的安装包即可

  ![image-20200504122034197](http://image.beloved.ink/Typora/image-20200504122034197.png)

- 开启Redis。允许服务即可

  ![image-20200504122331529](http://image.beloved.ink/Typora/image-20200504122331529.png)

- 使用客户端连接Redis

  ![image-20200504122726915](http://image.beloved.ink/Typora/image-20200504122726915.png)

# 3、Linux安装

- 下载地址：https://redis.io/

![image-20200504123022080](http://image.beloved.ink/Typora/image-20200504123022080.png)

- 上传压缩文件并解压

  我是准备安装在`/usr/local/redis`下，文件上传在此目录

  ![image-20200504133310910](http://image.beloved.ink/Typora/image-20200504133310910.png)

- **注意：安装redis需要gcc环境**

  ```bash
  gcc -v  # 查看版本
  
  yum -y install gcc  # 安装
  ```

- 进入解压文件目录使用make对解压的Redis文件进行编译

  <img src="http://image.beloved.ink/Typora/image-20200504133617027.png" alt="image-20200504133617027" style="zoom:150%;" />

  编译后的文件

  ![image-20200504133758397](http://image.beloved.ink/Typora/image-20200504133758397.png)

- 编译成功后，进入src文件夹，执行`make install`进行Redis安装。

  **报错**

  ![image-20200504134128369](http://image.beloved.ink/Typora/image-20200504134128369.png)

  **解决办法：**

  ```bash
  # 查看gcc版本是否在5.3以上，centos7.6默认安装4.8.5
  gcc -v
  # 升级gcc到5.3及以上,如下：
  升级到gcc 9.3：
  yum -y install centos-release-scl
  yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils
  scl enable devtoolset-9 bash
  需要注意的是scl命令启用只是临时的，退出shell或重启就会恢复原系统gcc版本。
  如果要长期使用gcc 9.3的话：
  
  echo "source /opt/rh/devtoolset-9/enable" >>/etc/profile
  这样退出shell重新打开就是新版的gcc了
  以下其他版本同理，修改devtoolset版本号即可。
  ```

  **重新安装，如果所示安装成功**

  ![image-20200504135017727](http://image.beloved.ink/Typora/image-20200504135017727.png)

- **为了方便管理，将Redis文件中的conf配置文件和常用命令移动到统一文件中**

  在`redis`创建`bin`和`etc`文件

  ![image-20200504140256987](http://image.beloved.ink/Typora/image-20200504140256987.png)

- 回到刚刚安装目录，找到`redis.conf`，将其复制移动到` /usr/local/redis/ect` 下

  ```bash
  cp redis.conf /usr/local/redis/etc/
  ```

  ![image-20200504140446042](http://image.beloved.ink/Typora/image-20200504140446042.png)

- 进入src目录,将常用命令移动到`bin`目录下

  ![image-20200504140605029](http://image.beloved.ink/Typora/image-20200504140605029.png)

- 修改配置文件

  **Redis默认不是后台启动**

  使用`vim`命令修改`redis.cong`配置文件

  ![image-20200504140916278](http://image.beloved.ink/Typora/image-20200504140916278.png)

  ![image-20200504140901340](http://image.beloved.ink/Typora/image-20200504140901340.png)

- 进入`bin`目录通过指定配置文件启动

  ```bash
  ./redis-server /usr/local/redis/etc/redis.conf
  ```

  ![image-20200504141358145](http://image.beloved.ink/Typora/image-20200504141358145.png)

- 使用客户端连接测试

  ```bash
  ./redis-cli -p 6379
  ```

  ![image-20200504141734319](http://image.beloved.ink/Typora/image-20200504141734319.png)

- 查看redis进程是否开启

  ```bash
  ps -ef|grep redis
  ```

  ![image-20200504141924557](http://image.beloved.ink/Typora/image-20200504141924557.png)

- 关闭Redis

  ```bash
  shutdown # 关闭
  exit     # 退出
  ```

  

  ![image-20200504142153129](http://image.beloved.ink/Typora/image-20200504142153129.png)

![image-20200504142251386](http://image.beloved.ink/Typora/image-20200504142251386.png)

# 4、测试性能

**redis-benchmark**：是一个官方自带的压力测试工具

redis 性能测试工具可选参数如下所示：

| 序号 | 选项      | 描述                                       | 默认值    |
| :--- | :-------- | :----------------------------------------- | :-------- |
| 1    | **-h**    | 指定服务器主机名                           | 127.0.0.1 |
| 2    | **-p**    | 指定服务器端口                             | 6379      |
| 3    | **-s**    | 指定服务器 socket                          |           |
| 4    | **-c**    | 指定并发连接数                             | 50        |
| 5    | **-n**    | 指定请求数                                 | 10000     |
| 6    | **-d**    | 以字节的形式指定 SET/GET 值的数据大小      | 2         |
| 7    | **-k**    | 1=keep alive 0=reconnect                   | 1         |
| 8    | **-r**    | SET/GET/INCR 使用随机 key, SADD 使用随机值 |           |
| 9    | **-P**    | 通过管道传输 <numreq> 请求                 | 1         |
| 10   | **-q**    | 强制退出 redis。仅显示 query/sec 值        |           |
| 11   | **--csv** | 以 CSV 格式输出                            |           |
| 12   | **-l**    | 生成循环，永久执行测试                     |           |
| 13   | **-t**    | 仅运行以逗号分隔的测试命令列表。           |           |
| 14   | **-I**    | Idle 模式。仅打开 N 个 idle 连接并等待。   |           |

**测试**

```bash
# 测试：100个并发请求 100000请求
redis-benchmark -h localhost -p 6379 -c 100 -n 100000
```

![image-20200504143650119](http://image.beloved.ink/Typora/image-20200504143650119.png)

**查看分析：SET**

![image-20200504144643872](http://image.beloved.ink/Typora/image-20200504144643872.png)

# 5、基础知识

**Redis默认有16个数据库，默认使用第0个。可以在配置文件查看**

![image-20200504145744009](http://image.beloved.ink/Typora/image-20200504145744009.png)

**可以使用Select切换数据库**

```bash
127.0.0.1:6379> select 6   # 切换数据库
OK 
127.0.0.1:6379[6]> dbsize  # 查看数据库大小
(integer) 0
```

![image-20200504150030254](http://image.beloved.ink/Typora/image-20200504150030254.png)

**查看所得key**

```
keys *
```

![image-20200504150126927](http://image.beloved.ink/Typora/image-20200504150126927.png)

**清除当前数据库**

```
flushdb
```

![image-20200504150243970](http://image.beloved.ink/Typora/image-20200504150243970.png)

**清除全部数据库的内容**

```
flushall
```

![image-20200504150428986](http://image.beloved.ink/Typora/image-20200504150428986.png)

> 为什么Redis是单线程还这么块？

redis是单线程，线程安全

redis可以能够快速执行的原因：

- 绝大部分请求是纯粹的内存操作（非常快速）
-  采用单线程,避免了不必要的上下文切换和竞争条件
-  非阻塞IO - IO多路复用（[IO 多路复用是什么意思？](https://www.zhihu.com/question/32163005)）

IO多路复用中有三种方式：select,poll,epoll。需要注意的是，select,poll是线程不安全的，epoll是线程安全的

redis内部实现采用epoll，采用了epoll+自己实现的简单的事件框架。epoll中的读、写、关闭、连接都转化成了事件，然后利用epoll的多路复用特性，绝不在io上浪费一点时间 这3个条件不是相互独立的，特别是第一条，如果请求都是耗时的，采用单线程吞吐量及性能可想而知了。应该说redis为特殊的场景选择了合适的技术方案。

# 6、五大数据类型

**官方介绍：**

![image-20200504152251525](http://image.beloved.ink/Typora/image-20200504152251525.png)

Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作==数据库==、==缓存==和==消息中间件==。 它支持多种类型的数据结构，如 [字符串（strings）](http://www.redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://www.redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://www.redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://www.redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://www.redis.cn/topics/data-types-intro.html#sorted-sets) 与范围查询， [bitmaps](http://www.redis.cn/topics/data-types-intro.html#bitmaps)， [hyperloglogs](http://www.redis.cn/topics/data-types-intro.html#hyperloglogs) 和 [地理空间（geospatial）](http://www.redis.cn/commands/geoadd.html) 索引半径查询。 Redis 内置了 [复制（replication）](http://www.redis.cn/topics/replication.html)，[LUA脚本（Lua scripting）](http://www.redis.cn/commands/eval.html)， [LRU驱动事件（LRU eviction）](http://www.redis.cn/topics/lru-cache.html)，[事务（transactions）](http://www.redis.cn/topics/transactions.html) 和不同级别的 [磁盘持久化（persistence）](http://www.redis.cn/topics/persistence.html)， 并通过 [Redis哨兵（Sentinel）](http://www.redis.cn/topics/sentinel.html)和自动 [分区（Cluster）](http://www.redis.cn/topics/cluster-tutorial.html)提供高可用性（high availability）。

## 6.1、Redis-Key

```bash
127.0.0.1:6379> keys *  # 查看当前数据库中的key
1) "name"
2) "age"
127.0.0.1:6379> exists name  # 判断是否存在某个值   返回1存在0不存在
(integer) 1
127.0.0.1:6379> exists name2
(integer) 0
127.0.0.1:6379> del name  # 移除当前key
(integer) 1
127.0.0.1:6379> move name 1  # 移动当前值到指定数据库
(integer) 1
127.0.0.1:6379> expire age 10  # 设置key的过期时间 单位是秒
(integer) 1
127.0.0.1:6379> ttl age  # 查看当前key的剩余时间
(integer) 6
127.0.0.1:6379> ttl age  # 返回负数已经过期
(integer) -2
127.0.0.1:6379> type age  # 查看当前key的类型
string
127.0.0.1:6379> type name
string

```

**更多命令可以在Redis官网查看**

![image-20200504154154237](http://image.beloved.ink/Typora/image-20200504154154237.png)

## 6.2、String

```bash
####################################################################################
127.0.0.1:6379> set k1 v1      # 设置值
OK
127.0.0.1:6379> get k1         # 获取值
"v1"
127.0.0.1:6379> keys *         # 获取当前所有的key
1) "k1"
127.0.0.1:6379> exists k1      # 判断某个key是否存在
(integer) 1
127.0.0.1:6379> append k1 v2   # 追加字符串
(integer) 4
127.0.0.1:6379> get k1
"v1v2"
127.0.0.1:6379> append k2 v2   # 追加字符串，如果当前的key不存在，就新建
(integer) 2
127.0.0.1:6379> keys *
1) "k1"
2) "k2"
127.0.0.1:6379> strlen k1      # 获取字符串长度
(integer) 4
127.0.0.1:6379> append k1 v3
(integer) 6
127.0.0.1:6379> strlen k1
(integer) 6
127.0.0.1:6379> get k1
"v1v2v3"
####################################################################################
127.0.0.1:6379> set i 1       # 设置初始值 1
OK
127.0.0.1:6379> get i
"1"
127.0.0.1:6379> incr i        # 自增1
(integer) 2
127.0.0.1:6379> incr i
(integer) 3
127.0.0.1:6379> get i
"3"
127.0.0.1:6379> decr i       # 自减1
(integer) 2
127.0.0.1:6379> get i
"2"
127.0.0.1:6379> incrby i 10  # 自增 步长10
(integer) 12
127.0.0.1:6379> get i
"12"
127.0.0.1:6379> decrby i 5   # 自建 步长 5
(integer) 7
127.0.0.1:6379> get i
"7"
####################################################################################
# 字符串范围range
127.0.0.1:6379> set k1 abcdefg   # 设置初始值
OK
127.0.0.1:6379> get k1
"abcdefg"
127.0.0.1:6379> getrange k1 0 3  # 截取字符串[0,3]
"abcd"
127.0.0.1:6379> getrange k1 0 -1 # 尾数负数 获取整个值
"abcdefg"
127.0.0.1:6379> setrange k1 1 xxx  # 替换指定位置开始的字符串
(integer) 7
127.0.0.1:6379> get k1
"axxxefg"
####################################################################################
# setex  设置key-value并设置过期时间 单位：秒
# setnx  当key不存在时在设置  (在分布式锁中经常使用)
127.0.0.1:6379> setex k1 10 abc # 设置k1的值abc  10秒后过期
OK
127.0.0.1:6379> ttl k1  		# 查看剩余时间
(integer) 7
127.0.0.1:6379> get k1
"abc"
127.0.0.1:6379> get k1
(nil)
127.0.0.1:6379> setnx k1 abc 	# 如果k1不存在  创建k1
(integer) 1
127.0.0.1:6379> get k1
"abc"
127.0.0.1:6379> setnx k1 aaa   # 如果k1存在    创建失败
(integer) 0
127.0.0.1:6379> get k1
"abc"
####################################################################################
# mset  批量创建
# mget  批量获取
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3   # 同时创建多个值
OK
127.0.0.1:6379> keys *
1) "k1"
2) "k2"
3) "k3"
127.0.0.1:6379> mget k1 k2 k3            # 同时获取多个值
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> msetnx k1 v1 k4 v4		# 如果不存在同时创建多个值
(integer) 0								# 原子性，要么一起成功，要么一起失败
127.0.0.1:6379> msetnx k4 v4 k5 v5		
(integer) 1
127.0.0.1:6379> keys *
1) "k4"
2) "k1"
3) "k5"
4) "k2"
5) "k3"

# 对象
set user:1:{name:张三,age:20}   # 设置一个user:1对象，值为json字符串保存对象
# user:id:{filed}
127.0.0.1:6379> mset user:1:name aaa user:1:age 20 
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "aaa"
2) "20"
127.0.0.1:6379> keys *
1) "user:1:name"
2) "user:1:age"
####################################################################################
getste # 先get然后set
127.0.0.1:6379> getset k1 aaa  # 如果不存在则返回nil，然后创建新的k-v
(nil)
127.0.0.1:6379> get k1
"aaa"
127.0.0.1:6379> getset k1 bbb  # 如果存在，则先返回原来的值，然后更新为新的值
"aaa"
127.0.0.1:6379> get k1
"bbb"
127.0.0.1:6379> 
```

## 6.3、List

在redis里可以吧list实现为：栈、队列、阻塞队列

**所有的list命令都是以：l开头的**

```bash
####################################################################################
127.0.0.1:6379> lpush list a   		# 将一个值或多个值，插入到列表的头部（左）
(integer) 1
127.0.0.1:6379> lpush list b c
(integer) 3
127.0.0.1:6379> keys *
1) "list"
127.0.0.1:6379> lrange list 0 -1	# 获取list中的值
1) "c"
2) "b"
3) "a"
127.0.0.1:6379> lrange list 0 1     # 通过区间获取指定的值
1) "c"
2) "b"
127.0.0.1:6379> rpush list 1 2 3
(integer) 6
127.0.0.1:6379> lrange list 0 -1	# 将一个值或多个值，插入到列表的尾部（右）
1) "c"
2) "b"
3) "a"
4) "1"
5) "2"
6) "3"
####################################################################################
# 移除  
127.0.0.1:6379> lrange list 0 -1
1) "c"
2) "b"
3) "a"
4) "1"
5) "2"
6) "3"
127.0.0.1:6379> lpop list 	# 移除列表的第一个元素
"c"
127.0.0.1:6379> rpop list   # 移除列表的最后一个元素
"3"
127.0.0.1:6379> lrange list 0 -1
1) "b"
2) "a"
3) "1"
4) "2"
####################################################################################
# lindex 通过下标获取值
127.0.0.1:6379> lrange list 0 -1
1) "b"
2) "a"
3) "1"
4) "2"
127.0.0.1:6379> lindex list 0		# 获取第一个值
"b"
127.0.0.1:6379> lindex list 1		# 获取指定的值
"a"
127.0.0.1:6379> lindex list -1		# 获取最后一个值
"2"
####################################################################################
# llen  获取列表的长度
127.0.0.1:6379> lrange list 0 -1
1) "b"
2) "a"
3) "1"
4) "2"
127.0.0.1:6379> llen list		# 获取列表的长度
(integer) 4
####################################################################################
# lrem 移除指定的值
127.0.0.1:6379> RPUSH list a a a b c 
(integer) 5
127.0.0.1:6379> lrange list 0 -1     	# 列表是可以存在重复值
1) "a"
2) "a"
3) "a"
4) "b"
5) "c"
127.0.0.1:6379> lrem list 1 a			# 移除list中指定个数的values
(integer) 1
127.0.0.1:6379> lrange list 0 -1
1) "a"
2) "a"
3) "b"
4) "c"
127.0.0.1:6379> lrem list 2 a
(integer) 2
127.0.0.1:6379> lrange list 0 -1
1) "b"
2) "c"
127.0.0.1:6379> lrem list 2 b			# 如果大于最大数量，就移除这个值全部
(integer) 1
127.0.0.1:6379> lrange list 0 -1
1) "c"
####################################################################################
# ltrim 截断
127.0.0.1:6379> rpush list a b c d e f g
(integer) 7
127.0.0.1:6379> lrange list 0 -1
1) "a"
2) "b"
3) "c"
4) "d"
5) "e"
6) "f"
7) "g"
127.0.0.1:6379> ltrim list 1 3          # 通过下标截取指定长度
OK
127.0.0.1:6379> lrange list 0 -1        # list被改变了，只剩下截取的部分
1) "b"
2) "c"
3) "d"
127.0.0.1:6379>
####################################################################################
# rpoplpush 移除列表的最后一个元素，并移动到新的列表中
127.0.0.1:6379> rpush list a b c
(integer) 3
127.0.0.1:6379> lrange list 0 -1
1) "a"
2) "b"
3) "c"
127.0.0.1:6379> rpoplpush list list2   # 移除列表的最后一个元素，并移动到新的列表中
"c"
127.0.0.1:6379> lrange list 0 -1   	   # 查看原来的列表 
1) "a"
2) "b"
127.0.0.1:6379> lrange list2 0 -1		# 查看目标列表
1) "c"
####################################################################################
# lset   将列表中指定下标的值替换成另外一个值，更新操作
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> exists list            # 判断列表是否存在
(integer) 0
127.0.0.1:6379> lset list 0 a		   # 如果不存在列表，更新会报错
(error) ERR no such key
127.0.0.1:6379> rpush list a
(integer) 1
127.0.0.1:6379> exists list 			# 判断列表是否存在
(integer) 1
127.0.0.1:6379> lset list 0 b			# 如果存在，更新指定下标的值
OK
127.0.0.1:6379> lrange list 0 0
1) "b"
127.0.0.1:6379> lset list 1 a			# 下标不存在，报错
(error) ERR index out of range
####################################################################################
# linsert   将某个值插入到列表中某个元素的前面或后面
127.0.0.1:6379> rpush list a b c 
(integer) 3
127.0.0.1:6379> lrange list 0 -1
1) "a"
2) "b"
3) "c"
127.0.0.1:6379> linsert list before b 1      # 插入到前面
(integer) 4
127.0.0.1:6379> lrange list 0 -1
1) "a"
2) "1"
3) "b"
4) "c"
127.0.0.1:6379> linsert list after b 2		# 插入到后面
(integer) 5
127.0.0.1:6379> lrange list 0 -1
1) "a"
2) "1"
3) "b"
4) "2"
5) "c"
####################################################################################
```

- list实际上是一个链表，before/after、left、right都可以插入值
- 如果key不存在，创建新的链表
- 如果key存在，新增内容
- 如果移除了所有的值，空链表，也代表不存在
- 在两边插入或改代值，效率最高！中间元素，相对效率会低

## 6.4、Set

**set的值不能重复**

```bash
####################################################################################
127.0.0.1:6379> sadd set a        # 将一个或多个值添加到set集合中
(integer) 1
127.0.0.1:6379> sadd set b c
(integer) 2
127.0.0.1:6379> sadd set a        # 如果存在，则返回0
(integer) 0
127.0.0.1:6379> smembers set	  # 查看set集合中的所有值
1) "c"
2) "a"
3) "b"
127.0.0.1:6379> sismember set d   # 判断某个值是否存在在set集合中
(integer) 0						  # 0不存在
127.0.0.1:6379> sismember set a
(integer) 1						  # 1存在
127.0.0.1:6379> scard set  		  # 获取set集合中元素内容的个数
(integer) 3
####################################################################################
# srem 移除元素
127.0.0.1:6379> smembers set
1) "c"
2) "a"
3) "b"
127.0.0.1:6379> srem set a b     # 移除一个或多个元素
(integer) 2
127.0.0.1:6379> smembers set
1) "c"
####################################################################################
# 随机
127.0.0.1:6379> sadd set a b c d
(integer) 4
127.0.0.1:6379> smembers set
1) "c"
2) "a"
3) "d"
4) "b"
127.0.0.1:6379> srandmember set      # 随机抽选一个元素
"c"
127.0.0.1:6379> srandmember set
"b"
127.0.0.1:6379> srandmember set
"a"
127.0.0.1:6379> srandmember set 2    # 随机抽选指定个数元素
1) "b"
2) "d"
127.0.0.1:6379> srandmember set 2
1) "c"
2) "b"
####################################################################################
127.0.0.1:6379> smembers set
1) "c"
2) "a"
3) "d"
4) "b"
127.0.0.1:6379> spop set           # 随机删除一个元素
"a"
127.0.0.1:6379> smembers set
1) "c"
2) "d"
3) "b"
127.0.0.1:6379> spop set 2		   # 随机删除指定个数的元素
1) "c"
2) "b"
127.0.0.1:6379> smembers set
1) "d"
####################################################################################
# smove 将一个指定的值移动到另一个set集合
127.0.0.1:6379> sadd set a b
(integer) 2
127.0.0.1:6379> sadd set2 c d
(integer) 2
127.0.0.1:6379> smove set set2 a    # 将一个指定的值移动到另一个set集合
(integer) 1
127.0.0.1:6379> smembers set
1) "b"
127.0.0.1:6379> smembers set2
1) "c"
2) "a"
3) "d"
####################################################################################
数字集合类:
 - 差集 sdiff    以前面的最基准
 - 交集 sinter
 - 并集 sunion
127.0.0.1:6379> sadd set1 a b c
(integer) 3
127.0.0.1:6379> sadd set2 c d e
(integer) 3
127.0.0.1:6379> sdiff set1 set2   # 差集
1) "a"
2) "b"
127.0.0.1:6379> sdiff set2 set1
1) "e"
2) "d"
127.0.0.1:6379> sinter set1 set2  # 交集
1) "c"
127.0.0.1:6379> sunion set1 set2  # 并集
1) "a"
2) "b"
3) "d"
4) "c"
5) "e"
127.0.0.1:6379>
####################################################################################
```

## 6.5、Hash

Map集合：key-map   value是map集合

```bash
####################################################################################
127.0.0.1:6379> hset user name Beloved   # set一个具体的值
(integer) 1
127.0.0.1:6379> hget user name			 # 获取以一个字段值
"Beloved"
127.0.0.1:6379> keys *
1) "user"
127.0.0.1:6379> hmset user name Beloved age 20	# set多个key-value  如果存在则覆盖
OK
127.0.0.1:6379> hmget user name age				# 获取多个字段值
1) "Beloved"
2) "20"
127.0.0.1:6379> hgetall user					# 获取某个key的全部字段值
1) "name"										# key
2) "Beloved"									# value
3) "age"
4) "20"
127.0.0.1:6379> hdel user age     			    # 删除指定的key的字段key  value页就删除
(integer) 1
127.0.0.1:6379> hgetall user
1) "name"
2) "Beloved"
####################################################################################
# hlen	 获取hash表的字段数量
127.0.0.1:6379> hmset user name Beloved age 20
OK
127.0.0.1:6379> hgetall user
1) "name"
2) "Beloved"
3) "age"
4) "20"
127.0.0.1:6379> hlen user    # 获取hash表的字段数量
(integer) 2
####################################################################################
127.0.0.1:6379> hgetall user
1) "name"
2) "Beloved"
3) "age"
4) "20"
127.0.0.1:6379> hexists user age   # 判断hash的key中指定字段是否存在
(integer) 1
127.0.0.1:6379> hexists user tel
(integer) 0
####################################################################################
# hkeys 获取key中所有的field
# hvals 获取key中所有的value
127.0.0.1:6379> hkeys user
1) "name"
2) "age"
127.0.0.1:6379> hvals user
1) "Beloved"
2) "20"
###################################################################################
127.0.0.1:6379> hget user age
"20"
127.0.0.1:6379> hincrby user age 2       # 可以只等增量   如果是负数，就减
(integer) 22
127.0.0.1:6379> hincrby user age -2
(integer) 20
127.0.0.1:6379> hsetnx user tel 110		 # 如果不存在则可以设置
(integer) 1
127.0.0.1:6379> hsetnx user tel 120      # 如果存在不能设置
(integer) 0
###################################################################################
```

## 6.6、Zset

**Zset有序集合**

在set的基础上，增加了一个排序set k v      zset k score v

```bash
###################################################################################
127.0.0.1:6379> zadd zset 1 a                      	# 添加一个值
(integer) 1
127.0.0.1:6379> zadd zset 2 b 3 c 4 d				# 添加多个值
(integer) 3
127.0.0.1:6379> zrange zset 0 -1 					# 指定区间获取值
1) "a"
2) "b"
3) "c"
4) "d"
127.0.0.1:6379> zrange zset 0 -1 withscores			# 指定区间获取值，带上序号
1) "a"
2) "1"
3) "b"
4) "2"
5) "c"
6) "3"
7) "d"
8) "4"
###################################################################################
# 排序   
# zrangebyscore  key min max   升序
# zrevrange key start stop 降序   不能使用 inf
# -inf无穷小   +inf无穷大
127.0.0.1:6379> zadd salary 2500 zhangsan 3000 lisi 1500 xiaoming 1800 xiaohong
(integer) 4
127.0.0.1:6379> zrangebyscore salary -inf +inf   # 显示全部，从小到大
1) "xiaoming"
2) "xiaohong"
3) "zhangsan"
4) "lisi"
127.0.0.1:6379> zrevrange salary 0 -1         # 从大到小排序
1) "lisi"
2) "zhangsan"
3) "xiaohong"
4) "xiaoming"
127.0.0.1:6379> zrangebyscore salary -inf +inf withscores   # 显示全部，并附带参数
1) "xiaoming"
2) "1500"
3) "xiaohong"
4) "1800"
5) "zhangsan"
6) "2500"
7) "lisi"
8) "3000"
127.0.0.1:6379> zrangebyscore salary 1800 3000       # 指定区间
1) "xiaohong"
2) "zhangsan"
3) "lisi"
###################################################################################
# zrem 删除元素   一个或多个
# zcard 获取集合中的元素个数
127.0.0.1:6379> zrange salary 0 -1
1) "xiaoming"
2) "xiaohong"
3) "zhangsan"
4) "lisi"
127.0.0.1:6379> zrem salary xiaohong xiaoming    # 删除元素   一个或多个
(integer) 2
127.0.0.1:6379> zrange salary 0 -1
1) "zhangsan"
2) "lisi"
127.0.0.1:6379> zcard salary					# 获取集合中的元素个数
(integer) 2
###################################################################################
# zcount key min max  获取成员数量
127.0.0.1:6379> zadd zset 1 a 2 b 3 c 4 d 5 e 6 f
(integer) 6
127.0.0.1:6379> zcount zset -inf +inf   # 获取全部的成员数量
(integer) 6
127.0.0.1:6379> zcount zset +inf -inf 
(integer) 0
127.0.0.1:6379> zcount zset 2 5         # 获取指定区间的成员数量
(integer) 4
###################################################################################
```

# 7、三种特殊数据类型

## 7.1、geospatial地理位置

Redis的Geo在Redis3.2版本退出

可以用于计算地理位置的信息，两地之间的距离，周围的人

官方文档：http://www.redis.cn/commands/geoadd.html

查询城市坐标：http://www.hao828.com/chaxun/zhongguochengshijingweidu/

![image-20200505154517545](http://image.beloved.ink/Typora/image-20200505154517545.png)

### geoadd

```bash
# geoadd 添加地理位置
# 规则：两级无法添加
# 参数 key value(纬度、经度、名称)
# 有效的经度从-180度到180度。
# 有效的纬度从-85.05112878度到85.05112878度。
# 当坐标位置超出上述指定范围时，该命令将会返回一个错误。
127.0.0.1:6379> geoadd china:city 116.408 39.904 beijing
(integer) 1
127.0.0.1:6379> geoadd china:city 121.445 31.213 shanghai
(integer) 1
127.0.0.1:6379> geoadd china:city 114.109 22.544 shenzhen 108.969 34.285 xian
(integer) 2
127.0.0.1:6379> geoadd china:city 120.165 30.319 hangzhou 114.279 30.573 wuhan
(integer) 2
127.0.0.1:6379> 
```

### geopos

```bash
127.0.0.1:6379> geopos china:city xian    # 获取指定城市的经纬度
1) 1) "108.96899789571762085"
   2) "34.28499959898385896"
127.0.0.1:6379> geopos china:city beijing shanghai 
1) 1) "116.40800267457962036"
   2) "39.90399988166036138"
2) 1) "121.44499808549880981"
   2) "31.213001199663303"
127.0.0.1:6379>
```

### geodist

返回两个给定位置之间的距离。

 如果两个位置之间的其中一个不存在, 那么命令返回空值。

 指定单位的参数 unit 必须是以下单位的其中一个: 

- m 表示单位为米 
- km 表示单位为千米 
- mi 表示单位为英里 
- ft 表示单位为英尺 

如果用户没有显式地指定单位参数, 那么 GEODIST 默认使用米作为单位

```bash
127.0.0.1:6379> geodist china:city beijing xian km   # 查看北京到西安的直线距离
"908.2962"
127.0.0.1:6379> geodist china:city beijing shanghai km   # 查看北京到上海的直线距离
"1068.2320"
```

### georadius

**以给定的经纬度为中心，找出某一半径内的元素**

以给定的经纬度为中心， 返回键包含的位置元素当中， 与中心的距离不超过给定最大距离的所有位置元素。

范围可以使用以下其中一个单位：

- **m** 表示单位为米。
- **km** 表示单位为千米。
- **mi** 表示单位为英里。
- **ft** 表示单位为英尺。

在给定以下可选项时， 命令会返回额外的信息：

- `WITHDIST`: 在返回位置元素的同时， 将位置元素与中心之间的距离也一并返回。 距离的单位和用户给定的范围单位保持一致。
- `WITHCOORD`: 将位置元素的经度和维度也一并返回。
- `WITHHASH`: 以 52 位有符号整数的形式， 返回位置元素经过原始 geohash 编码的有序集合分值。 这个选项主要用于底层应用或者调试， 实际中的作用并不大。

命令默认返回未排序的位置元素。 通过以下两个参数， 用户可以指定被返回位置元素的排序方式：

- `ASC`: 根据中心的位置， 按照从近到远的方式返回位置元素。
- `DESC`: 根据中心的位置， 按照从远到近的方式返回位置元素。

在默认情况下， GEORADIUS 命令会返回所有匹配的位置元素。 虽然用户可以使用 **COUNT ``** 选项去获取前 N 个匹配元素， 但是因为命令在内部可能会需要对所有被匹配的元素进行处理， 所以在对一个非常大的区域进行搜索时， 即使只使用 `COUNT` 选项去获取少量元素， 命令的执行速度也可能会非常慢。 但是从另一方面来说， 使用 `COUNT` 选项去减少需要返回的元素数量， 对于减少带宽来说仍然是非常有用的。

**返回值**

[bulk-string-reply](http://www.redis.cn/topics/protocol.html#bulk-string-reply), 具体的:

- 在没有给定任何 `WITH` 选项的情况下， 命令只会返回一个像 [“New York”,”Milan”,”Paris”] 这样的线性（linear）列表。
- 在指定了 `WITHCOORD` 、 `WITHDIST` 、 `WITHHASH` 等选项的情况下， 命令返回一个二层嵌套数组， 内层的每个子数组就表示一个元素。

在返回嵌套数组时， 子数组的第一个元素总是位置元素的名字。 至于额外的信息， 则会作为子数组的后续元素， 按照以下顺序被返回：

1. 以浮点数格式返回的中心与位置元素之间的距离， 单位与用户指定范围时的单位一致。
2. geohash 整数。
3. 由两个元素组成的坐标，分别为经度和纬度

```bash
127.0.0.1:6379> georadius china:city 110 30 500 km   # 以100，30经纬度为中心，获取半径500km内的城市
1) "xian"
2) "wuhan"
127.0.0.1:6379> georadius china:city 110 30 500 km withdist   # 显示到中心距离的直线距离
1) 1) "xian"
   2) "486.3850"
2) 1) "wuhan"
   2) "415.8636"
127.0.0.1:6379> georadius china:city 110 30 500 km withcoord  # 显示目标的经纬度
1) 1) "xian"
   2) 1) "108.96899789571762085"
      2) "34.28499959898385896"
2) 1) "wuhan"
   2) 1) "114.27899926900863647"
      2) "30.57299931525717795"
127.0.0.1:6379> georadius china:city 110 30 500 km withdist withcoord  # 显示目标的经纬度和到中心的直线距离
1) 1) "xian"
   2) "486.3850"
   3) 1) "108.96899789571762085"
      2) "34.28499959898385896"
2) 1) "wuhan"
   2) "415.8636"
   3) 1) "114.27899926900863647"
      2) "30.57299931525717795"
127.0.0.1:6379> georadius china:city 110 30 500 km withdist withcoord count 1  # 数量查询
1) 1) "wuhan"
   2) "415.8636"
   3) 1) "114.27899926900863647"
      2) "30.57299931525717795"
127.0.0.1:6379> 
```

### georadiusbymember

**和georadius差不多，只不过是将坐标换成元素**

```bash
# 找出指定元素周围的其他元素
127.0.0.1:6379> georadiusbymember china:city beijing 1000 km
1) "beijing"
2) "xian"
127.0.0.1:6379> 
```

### geoHash

**返回一个或多个位置元素的geoHash表示**

**该命令返回11个字符的geoHash字符串**

```bash
# 将二维的经纬度转换为一维的字符串  如果字符串越接近越精确
127.0.0.1:6379> geohash china:city beijing 
1) "wx4g0bm9xh0"
127.0.0.1:6379> geohash china:city beijing xian
1) "wx4g0bm9xh0"
2) "wqj7p9ku9e0"
127.0.0.1:6379> 
```

### 原理

**geo的底层实现原理就是Zset，可以使用Zset命令操作geo**

```bash
127.0.0.1:6379> zrange china:city 0 -1    # 查询所有元素
1) "xian"
2) "shenzhen"
3) "wuhan"
4) "hangzhou"
5) "shanghai"
6) "beijing"
127.0.0.1:6379> zrem china:city xian     # 删除某一个元素
(integer) 1
127.0.0.1:6379> zrange china:city 0 -1
1) "shenzhen"
2) "wuhan"
3) "hangzhou"
4) "shanghai"
5) "beijing"
127.0.0.1:6379> 
```

## 7.2、Hyperloglogs

**官网介绍：**

HyperLogLog是一种概率数据结构，用于对唯一事物进行计数（从技术上讲，这是指估计集合的基数）。 通常，对唯一项目进行计数需要使用与要计数的项目数量成比例的内存量，因为您需要记住过去已经看到的元素，以避免多次对其进行计数。 但是，有一组算法会以内存换取精度：以Redis实施为例，您得出的带有标准误差的估计度量最终会小于1％。 该算法的神奇之处在于，您不再需要使用与计数的项目数量成比例的内存量，而是可以使用恒定数量的内存！ 在最坏的情况下为==12k字节==，如果您的HyperLogLog（从现在开始将它们称为HLL）看到的元素很少，则少得多。

Redis中的HLL尽管在技术上是不同的数据结构，但被编码为Redis字符串，因此您可以调用GET来序列化HLL，然后调用SET来将其反序列化回服务器。

从概念上讲，HLL API就像使用Set来执行相同的任务。 您可以将每个观察到的元素添加到集合中，并使用SCARD检查集合中的元素数量，这是唯一的，因为SADD不会重新添加现有元素。

尽管您并未真正将项目添加到HLL中，但由于数据结构仅包含不包含实际元素的状态，因此API相同：

每次看到新元素时，都可以使用PFADD将其添加到计数中。

到目前为止，每次您要检索添加到PFADD中的唯一元素的当前近似值时，都使用PFCOUNT。

> 简介

Redis 2.8.9 版本更新了Hyperloglog数据结构

Redis Hyperloglog 基数统计的算法

优点：占用内存是固定的，2^64不同的元素的基数，只需要12kb内存

```bash
127.0.0.1:6379> pfadd key a b c d e f b       # 创建第一组元素
(integer) 1
127.0.0.1:6379> pfcount key 				  # 统计基数数量
(integer) 6
127.0.0.1:6379> pfadd key2 f c q w r t
(integer) 1
127.0.0.1:6379> pfcount key2
(integer) 6
127.0.0.1:6379> pfmerge key3 key key2		  # 合并两组基数为新的一组基数 ，并集
OK
127.0.0.1:6379> pfcount key3
(integer) 10
127.0.0.1:6379>
```

## 7.3、Bitmaps

**位存储**

可以用来统计用户信息，活跃、不活跃、登录、未登录、打卡.......两个状态的，都可以使用Bitmaps

Bitmaps位图，数据结构。都是操作二进制位进行记录，只有0、1两个状态

**练习：做一个一周打卡系统。可以使用Bitmaps**

![image-20200505212630093](http://image.beloved.ink/Typora/image-20200505212630093.png)

```bash
127.0.0.1:6379> setbit sign 0 1  		# 周一 打卡
(integer) 0
127.0.0.1:6379> setbit sign 1 1			# 周二 打卡
(integer) 0
127.0.0.1:6379> setbit sign 2 0			# 周三 未打卡
(integer) 0
127.0.0.1:6379> setbit sign 3 0			# 周四 未打卡
(integer) 0
127.0.0.1:6379> setbit sign 4 1 		# 周五 打卡
(integer) 0
127.0.0.1:6379> setbit sign 5 1			# 周六 打卡
(integer) 0
127.0.0.1:6379> setbit sign 6 0			# 周天 未打卡
(integer) 0
#####################################################################################
# 查看某一天是否有打卡
127.0.0.1:6379> getbit sign 0  			# 查看周一是否打卡
(integer) 1
127.0.0.1:6379> getbit sign 2			# 查看周三是否打卡
(integer) 0
#####################################################################################
# 统计操作     
127.0.0.1:6379> bitcount sign           # 统计一周内的打卡天数
(integer) 4
```

# 8、事务

Redis事务本质：一组命令集合。一个事务中所有命令都会被序列化，在事务执行过程中，会按照顺序执行

一次性、顺序性、排他性

所有的命令在事务中，并没有直接被执行，只有发起执行命令的时候才会执行

==Redis事务没有隔离级别的概念==

==Redis单条命令是保证原子性的，但事务不保证原子性==

Redis的事务：

- 开始事务（multi）
- 命令入队（.........）
- 执行事务（exec）

## 8.1、正常执行事务

```bash
127.0.0.1:6379> multi				# 开启事务
OK
# 命令入队
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2 
QUEUED
127.0.0.1:6379> get k2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> exec				# 执行事务
1) OK
2) OK
3) "v2"
4) OK
127.0.0.1:6379> 
```

## 8.2、取消事务  **discard**

```bash
127.0.0.1:6379> multi			# 开始事务
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k4 v4
QUEUED
127.0.0.1:6379> discard			# 取消事务
OK
127.0.0.1:6379> get k4			# 事务队列中的命令没有被执行
(nil)
127.0.0.1:6379> 
```

## 8.3、异常

### 8.3.1、编译型异常

**代码有问题。事务中的所有命令都不会执行**

```bash
127.0.0.1:6379> multi				# 开启事务
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> getste k2 v2		# 错误命令
(error) ERR unknown command `getste`, with args beginning with: `k2`, `v2`, 
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> exec				# 执行事务时报错
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get k1				# 事务中正确的命令也没有执行
(nil)
```

### 8.3.2、运行时异常

比如：1/0

**事务执行时，只有错误的报错，其余命令正常执行，抛出错误异常**

```bash
127.0.0.1:6379> set k1 v1     # 初始化字符串
OK
127.0.0.1:6379> multi		  # 开启事务
OK
127.0.0.1:6379> incr k1		  # 对字符串自增   这里会失败
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED	
127.0.0.1:6379> exec		  # 执行事务   只有失败的命令抛出异常，其余命令执行成功
1) (error) ERR value is not an integer or out of range
2) OK
3) OK
127.0.0.1:6379> get k2
"v2"
```

## 8.4、监控Watch

**悲观锁：**

- 很悲观，认为什么时候都会出问题，无论做什么都上锁

**乐观锁：**

- 很乐观，认为什么时候都不会出问题，不上锁，更新数据的时候去判断一下，在此期间是否有人修改过数据
- 获取version
- 更新的时候比较 version

**测试：**

正常执行成功

**事务正常执行完毕，监控自动取消**

```bash
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> set out 0
OK
127.0.0.1:6379> watch money			# 监视money
OK
127.0.0.1:6379> multi				# 开启事务
OK
127.0.0.1:6379> decrby money 20
QUEUED
127.0.0.1:6379> 
127.0.0.1:6379> incrby out 20
QUEUED
127.0.0.1:6379> exec				# 事务正常结束，期间数据没有发生改变，正常执行成功
1) (integer) 80
2) (integer) 20
```

模拟多线程，另一个线程修改值。

**使用watch可以当作redis的乐观锁操作**

```bash
127.0.0.1:6379> watch money			# 监视 money
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> decrby money 20
QUEUED
127.0.0.1:6379> incrby out 20
QUEUED
127.0.0.1:6379> exec				# 执行之前，另外一个线程修改了money，导致事务执行失败
(nil)
127.0.0.1:6379> 
```

如果修改失败，获取最新的值，重新监视

**取消监视：unwatch**

```bash
127.0.0.1:6379> unwatch     		# 取消监视
OK
127.0.0.1:6379> watch money			# 获取最新的值，重新监视
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> decrby money 20
QUEUED
127.0.0.1:6379> incrby out 20
QUEUED
127.0.0.1:6379> exec			# 对比监视的值，是否发生改变，如果没有改变执行成功，变则失败
1) (integer) 980
2) (integer) 40
```

