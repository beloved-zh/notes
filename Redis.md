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



