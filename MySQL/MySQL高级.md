# 1、MySQL架构介绍

## 1.1、简介

**官网：https://www.mysql.com/**

MySQL是一个关系性数据库管理系统，由瑞典MySQL AB公司开发，目前属于Oracle公司

MySQL是一种关系型数据库管理系统，关系数据库将数据保存在不同的表中，而不是将所有数据放在一个大仓库内，这样就增加了速度并提高了灵活性。

MySQL所使用的 SQL 语言是用于访问数据库的最常用标准化语言。

MySQL 软件采用了双授权政策，分为社区版和商业版，由于其体积小、速度快、总体拥有成本低，尤其是开放源码这一特点，一般中小型网站的开发都选择 MySQL 作为网站数据库。

## 1.2、安装及配置

**centos7下使用yum源安装mysql5.7**

### 1.2.1、安装

- **下载yum源**

  ```bash
  wget 'https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm'
  ```

  ![image-20200927170216477](image-20200927170216477.png)

- **安装yum源**

  ```bash
  rpm -Uvh mysql57-community-release-el7-11.noarch.rpm
  ```

  ![image-20200927170355552](image-20200927170355552.png)

- **查看有哪些mysql版本**

  ```bash
  yum repolist all | grep mysql
  ```

  ![image-20200927170519855](image-20200927170519855.png)

- **安装MySQL**

  默认安装5.7

  ```bash
  yum install -y mysql-community-server
  ```

  ![image-20200927171226757](image-20200927171226757.png)

- **查看版本**

  ```bash
  mysqladmin --version
  ```

  ![image-20200927171517768](image-20200927171517768.png)

### 1.2.2、启动MySQL服务

**启动服务**

```bash
systemctl start mysqld
```

**设置开机自启动**

```bash
systemctl enable mysqld
systemctl daemon-reload
```

### 1.2.3、设置ROOT密码

mysql5.7的新特性之一就是在初始化的时候会生成一个自定义的密码，然后你需要找到这个密码，登录的时候输入。注意，输入密码的时候是不显示。
找到密码: 红框的地方就是密码

```bash
grep 'temporary password' /var/log/mysqld.log
```

![image-20200927172318474](image-20200927172318474.png)

**登录数据库**

```bash
mysql -uroot -p
```

![image-20200927172424972](image-20200927172424972.png)

**修改密码**

```bash
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
```

![image-20200927172557791](image-20200927172557791.png)

修改自定义密码时，由于自定义密码比较简单，就出现了不符合密码策略的问题。

必须修改两个全局参数：

```bash
set global validate_password_policy=0;
set global validate_password_length=1;
```

再次执行修改密码

```bash
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
```

### 1.2.4、设置远程登录

现在这样是无法在本地用工具登录访问的，现在要做两件事，一件事是开放3306端口；另一件事是配置远程可以访问。

**先设置刚才的密码可以远程登录，然后使用flush命令使配置立即生效。**

```bash
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
flush privileges;
```

**开放3306防火墙端口**

```bash
# 开放端口
firewall-cmd --zone=public --add-port=3306/tcp --permanent
# 重启防火墙
systemctl restart firewalld.service
```

**重启数据库**

```bash
systemctl restart mysqld
```

**远程连接测试**

![image-20200927173217122](image-20200927173217122.png)

### 1.2.5、修改字符编码集

由于我们是中文系统，而MYSQL默认的字符为latin1，所以容易出现乱码的情况，因此需要修改字符集的编码方式

**进入mysql，查看字符编码集**

```mysql
 show variables like '%char%';
```

![image-20200927190359313](image-20200927190359313.png)

MYSQL 5.7 开始 默认配置文件是：`/etc/my.cng`

```bash
vim /etc/my.cnf
```

**增加这几项**

```bash
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
[mysqld]
character_set_server = utf8
collation_server = utf8_general_ci
```

![image-20200927190603770](image-20200927190603770.png)

**重启MYSQL服务**

```bash
systemctl restart mysqld
```

**查看字符编码集**

```mysql
 show variables like '%char%';
```

![image-20200927190718320](image-20200927190718320.png)

**这里注意的是修改字符集只对之后的数据库生效，之前已经存在的数据库无效，所以建议安装完MYSQL后第一时间修改编码方式**

## 1.3、配置文件

**RPM安装MySql时的默认路径：**

- 数据文件：`/var/lib/mysql/`
- 配置文件模板：`/usr/share/mysql mysql`
- 客户端工具目录：`/usr/bin`
- 日志目录：`/var/log/pid`
- sock文件：`/var/lib/mysql/`
- 配置文件会放置在：`/etc`

**主要配置文件：**

- 二进制日志log-bin：主从复制
- 错误日志log-error：默认是关闭的，记录严重的警告和错误信息，每次启动和关闭的详细信息等。
- 查询日志log：默认关闭，记录查询的SQL语句，如果开启会降低MYSQL的整体性能，因为记录日志也是需要消耗系统资源
- 数据文件：
  - 两系统：
    - Windows：`安装目录\data`
    - Linux：默认路径：`/var/lib/mysql`
  - frm文件：存放表结构
  - myd文件：存放表数据
  - myi文件：存放表索引
- 如何配置：
  - Windows：`my.ini`文件
  - Linux：`/etc/my.cnf`文件

## 1.4、逻辑架构介绍

和其他数据库相比，MySQL它的架构可以在多种不同场景中应用并发挥良好作用。主要体现在存储引擎的架构上

插件式的存储引擎架构将查询处理和其他的系统任务以及数据的存储提取相分离。这种架构可以根据业务的需求和实际需求选择合适的存储引擎

**逻辑架构图：**

![img](20180831173911997)

- **连接层：**最上层是一些客户端和连接服务，包含本地sock通信和大多数基于客户端/服务端工具实现的类似于tcp/ip的通信。主要完成一些类似于连接处理，授权认证、及相关的安全方案。在该层上引入了连接池的概念，为通过认证安全接入的客户端提供线程。同样在该层上可以实现基于SSL的安全链接。服务器也会为安全接入的每个客户端验证它所具有的操作权限
- **服务层：**第二层架构主要完成大多少的核心服务功能，如SQL接口，并完成缓存的查询，SQL的分析和优化及部分内置函数的执行。所有跨存储引擎的功能也在这一层实现，如过程、函数等。在该层，服务器会解析查询并创建相应的内部解析数，并对其完成相应的优化如确认查询表的顺序，是否利用索引等，最后生成相应的执行操作。如果是select语句，服务器还会查询内部的缓存。如果缓存空间足够大，这样在解决大量读操作的环境中能够很好的提升系统的性能
- **引擎层：**存储引擎层，存储引擎真正的负责了MYSQL中数据的存储和提取，服务器通过API与存储引擎进行通信。不同的存储引擎具有的功能不同，这样可以根据自己的实际需要进行选取
- **存储层：**数据存储层，主要是将数据存储在运行于设备的文件系统上，并完成与存储引擎的交互

## 1.5、存储引擎

### 1.5.1、查看命令

**查看mysql提供了什么存储引擎**

```mysql
show engines;
```

![image-20200927210654297](image-20200927210654297.png)

**查看当前默认的存储引擎**

```mysql
show variables like '%storage_engine%';
```

![image-20200927210747100](image-20200927210747100.png)

### 1.5.2、MyISAM和InnoDB

| 对比项   | MyISAM                                                 | InnoDB                                                       |
| -------- | ------------------------------------------------------ | ------------------------------------------------------------ |
| 主外键   | 不支持                                                 | 支持                                                         |
| 事务     | 不支持                                                 | 支持                                                         |
| 行表锁   | 表锁，即使操作一条记录也会锁住整个表不适合高并发的操作 | 行锁，操作时只锁某一行，不对其它行有影响，适合高并发操作     |
| 缓存     | 只缓存索引，不缓存真实数据                             | 不仅缓存索引还要缓存真实数据，对内存要求较高，而且内存大小对性能有决定性的影响 |
| 表空间   | 小                                                     | 大                                                           |
| 关注点   | 性能                                                   | 事务                                                         |
| 默认安装 | Y                                                      | Y                                                            |

# 2、索引优化分析

## 2.1、SQL执行加载顺序

**手写**

```mysql
SELECT DISTINCT
	<select_list>
FROM
	<left_table> <join_type> JOIN <right_table> ON <join_condition>
WHERE 
	<where_condition>
GROUP BY
	<group_by_list>
HAVING
	<having_condition>
ORDER BY
	<order_by_condition>
LIMIT <limit_number>
```

**机读**

```mysql
FROM <left_table>
ON <join_condition>
<join_type> JOIN <right_table>
WHERE <where_condition>
GROUP BY <group_by_list>
HAVING <having_condition>
SELECT 
DISTINCT <select_list>
ORDER BY <order_by_condition>
LIMIT <limit_number>
```

**总结**

![image-20200928141024402](image-20200928141024402.png)

## 2.2、常见JOIN查询

**JOIN图解：**

<img src="sql-join.png" alt="img" style="zoom: 80%;" />

**创建数据：**

```mysql
CREATE TABLE `tb_dept` (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '部门主键',
  `deptName` varchar(30) DEFAULT NULL COMMENT '部门名称',
  `locAdd` varchar(40) DEFAULT NULL COMMENT '楼层',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;


CREATE TABLE `tb_emp` (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '员工主键',
  `name` varchar(20) DEFAULT NULL COMMENT '员工姓名',
  `deptId` int(11) DEFAULT NULL COMMENT '部门外键',
  PRIMARY KEY (`id`),
  KEY `fk_dept_id` (`deptId`)
  # CONSTRAINT `fk_dept_id` FOREIGN KEY (`deptId`) REFERENCES `tb_dept` (`id`) COMMENT '部门外键设置, 已经注释掉。'
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

INSERT INTO tb_dept(deptName,locAdd) VALUES ('开发部', '11');
INSERT INTO tb_dept(deptName,locAdd) VALUES ('人事部', '12');
INSERT INTO tb_dept(deptName,locAdd) VALUES ('销售部', '13');
INSERT INTO tb_dept(deptName,locAdd) VALUES ('研发部', '14');
INSERT INTO tb_dept(deptName,locAdd) VALUES ('市场部', '15');

INSERT INTO tb_emp(name,deptId) VALUES ('张三', '1');
INSERT INTO tb_emp(name,deptId) VALUES ('李四', '1');
INSERT INTO tb_emp(name,deptId) VALUES ('王五', '1');
INSERT INTO tb_emp(name,deptId) VALUES ('小明', '2');
INSERT INTO tb_emp(name,deptId) VALUES ('小马', '2');
INSERT INTO tb_emp(name,deptId) VALUES ('小红', '3');
INSERT INTO tb_emp(name,deptId) VALUES ('小丁', '4');
INSERT INTO tb_emp(name,deptId) VALUES ('小西', '51');
```

### 2.2.1、Inner JOIN

![img](1548731841-2663-INNER-JOIN.png)

```mysql
select * from tb_dept d inner join tb_emp e on d.id = e.deptId;
```

![image-20200928143542397](image-20200928143542397.png)

### 2.2.2、Left JOIN

![img](1548731843-2683-LEFT-JOIN.png)

```mysql
 select * from tb_dept d left join tb_emp e on d.id = e.deptId;
```



![image-20200928143717204](image-20200928143717204.png)

### 2.2.3、Right JOIN

![img](1548731845-3187-RIGHT-JOIN.png)

```mysql
select * from tb_dept d right join tb_emp e on d.id = e.deptId;
```

![image-20200928143845064](image-20200928143845064.png)

### 2.2.4、Left Excluding JOIN

![img](1548731849-9936-LEFT-EXCLUDING-JOIN.png)

```mysql
select * from tb_dept d left join tb_emp e on d.id = e.deptId where e.id is null;
```

![image-20200928144131241](image-20200928144131241.png)

### 2.2.5、Right Excluding JOIN

![img](1548731850-7833-RIGHT-EXCLUDING-JOIN.png)

```mysql
 select * from tb_dept d right join tb_emp e on d.id = e.deptId where d.id is null;
```

![image-20200928144544797](image-20200928144544797.png)

### 2.2.6、Outer JOIN

![img](1548731847-7074-FULL-OUTER-JOIN.png)

**MYSQL不支持**`FULL OUTER JOIN`关键字

```mysql
select * from tb_dept d full outer join tb_emp e on d.id = e.deptId;
```

![image-20200928145101658](image-20200928145101658.png)

但是可以通过左连接和右连接达到效果

**使用union（合并去重）关键字解决中间公有部分**

```mysql
select * from tb_dept d left join tb_emp e on d.id = e.deptId
union
select * from tb_dept d right join tb_emp e on d.id = e.deptId;
```

![image-20200928145252560](image-20200928145252560.png)

### 2.2.7、Outer Excluding JOIN

![img](1548731854-3464-OUTER-EXCLUDING-JOIN.png)

```mysql
select * from tb_dept d left join tb_emp e on d.id = e.deptId where e.id is null
union
select * from tb_dept d right join tb_emp e on d.id = e.deptId where d.id is null;
```

![image-20200928145634434](image-20200928145634434.png)

## 2.3、索引简介

### 2.3.1、简介

官方定义：索引（Index）是帮助MYSQL高效获取数据的数据结构。索引的目的是在于提高查询效率，可以类比字典。**索引是数据结构**

**排好序的快速查找数据结构：**

- 在数据之外，**数据库系统还维护着满足特定查找算法的数据结构**，这些数据结构以某种方式引用（指向）数据，这样就可以在数据结构上实现高级查找算法。这种数据结构，就是索引。

  ![image-20200928162046846](image-20200928162046846.png)

  - 为了加快Col2的查找，可以维护一个右边所示的二叉查找数，每个节点分别包含索引键值和一个指向对应数据记录的物理地址的指针，这样就可以运用二叉查找在一定复杂度内获取到相应数据，快速检索出符合条件的记录

索引本身也很大，不可能全部存储在内存中，因此索引是以索引文件的形式存储在磁盘上

**平常所说的索引，如果没有特别说明，都是指B树（多路搜素树，并不一定是二叉的）结构组织的索引。**其中聚集索引、次要索引、复合索引、前缀索引、唯一索引默认都是使用B+树索引，统称索引

### 2.3.2、优势劣势

**优势：**

- 提高数据检索效率，降低数据量的IO成本
- 通过索引列对数据进行排序，降低数据排序的成本，降低了CPU的消耗

**劣势：**

- 实际上索引也是一张表，该表保存了主键与索引字段，并指向实体表的记录，所以索引列也是要占用空间的
- 虽然索引大大提高了查询速度，同时却会降低更新表的速度，如对表进行INSERT、UPDATE、DELETE。因为更新表时，MYSQL不仅要保存数据，还要保存索引文件每次更新添加了索引列的字段，都会调整用为更新所带来的键值变化后的索引信息
- 索引只是提高效率的一个因素，如果有大数据量的表，就要花时间研究建立合适的索引，或优化

### 2.3.3、索引分类和创建

- **普通索引(normal)：**没有任何约束，主要用于提高查询效率
- **唯一索引(UNIQUE)：**在普通索引的基础上增加了数据唯一性的约束，可以有多个
- **主键索引(primary key)：**主键索引在唯一索引的基础上增加了不为空的约束，也就是 NOT NULL+UNIQUE，只能有一个
- **全文索引(FULLTEXT)：**MySQL 自带的全文索引只支持英文。

**基本语法：**

- **创建：**

  ```mysql
  -- 方式一
  CREATE [UNIQUE] INDEX 索引名 ON 表名(column_list);
  -- 方式二
  ALTER TABLE 表名 ADD [UNIQUE] INDEX 索引名 ON (column_list);
  ```

- **删除：**

  ```mysql
  DROP INDEX 索引名 ON 表名
  ```

- **查看：**

  ```mysql
  SHOW INDEX FROM 表名
  ```

- **ALTER命令：**

  ```mysql
  -- 添加一个主键，索引值必须是唯一的且不能为NULL
  ALTER TABLE 表名 ADD PRIMARY KEY(column_list);
  
  -- 创建索引的值必须是唯一的（除了NULL外，NULL可能会出现多次）
  ALTER TABLE 表名 ADD UNIQUE 索引名 (column_list);
  
  -- 添加普通索引，索引值可出现多次
  ALTER TABLE 表名 ADD INDEX 索引名 (column_list);
  
  -- 指定索引为FULLTEXT，用于全文检索
  ALTER TABLE 表名 ADD FULLTEXT 索引名 (column_list);
  ```

### 2.3.4、索引结构

- Hash索引

- full-text全文索引

- R-Tree索引

- BTree索引

  <img src="image-20200928170219796.png" alt="image-20200928170219796" style="zoom:80%;" />

初始化介绍：

一颗b+树，浅蓝色的块称之为一个磁盘块，每个磁盘块包含几个数据项（深蓝色所示）和指示（黄色所示），如磁盘块1包含数据项17和35，包含指针P1、P2、P3

P1表示小于17的磁盘块，P2表示在17和35之间的磁盘块，P3表示大于35的磁盘块

真实的数据存在于叶子节点，即3、5、9、10、13、15、28、19、29、36、60、75、79、90、99

非叶子节点不存储真实数据，只存储指引搜索方向的数据项，如17、35并不真实存在于数据表中。

查找过程：

如果要查找数据项29，那么首先会把磁盘1由磁盘加载到内存，此时发生一次IO，在内存中用二分查找确定29在17和35之间，锁定磁盘块1的P2指针，内存时间因为非常短（相比磁盘的IO）可以忽略不计，通过磁盘块1的P2指针的磁盘地址吧磁盘块3由磁盘加载到内存，发生第二次IO，29在26和30之间，磁盘锁定块3的P2指针，通过指针加载磁盘块8到内存，发生第三次IO，同时内存中做二分查找找到29，结束查询，总计三次IO

真实的情况是，3层的b+树可以表示上百万的数据，如果上百万的数据查找只需要三次IO，性能提高将是巨大的，如果没有索引，每个数据项都要发生一次IO，那么总共需要百万次IO，成本是非常高的

### 2.3.5、索引创建的情况

**适合条件：**

- 主键自动建立唯一索引
- 频繁作为查询条件的字段应该创建索引
- 查询中与其它表关联的字段，外键关系建立索引
- WHERE条件里用不到的字段不创建索引
- 单键/组合索引的选择：在高并发下创建组合索引
- 查询中排序的字段，排序字段若通过索引去访问将大大提高排序速度
- 查询中统计或者分组字段

**不适合条件：**

- 表记录太少
- 频繁更新的字段不适合创建索引。因为每次更新不单单是更新了记录还会更新索引
- 经常增删改的表
  - 提高了查询速度，同时却会降低更新表的速度，如对表进行INSERT、UPDATE、DELETE。因为更新表时，MYSQL不仅要保存数据，还要保存索引文件
- 数据重复且分布平均的表字段。如果某个数据列包含许多重复的内容，建立索引就没有太大的实际效果
  - 假如一个表由10万行记录，有一个字段A只有T和F两种值，且每个值的分布概率不约为50%，那么对这种表A字段建索引一般不会提高数据库的查询速度
  - 索引的选择性是指索引列中不同值的数目与表中记录数的比，如果一个表中有2000条记录，表索引列有1980个不同的值，那么这个索引的选择性就是1980/2000=0.99。一个索引的选择性接近于1，这个索引的效率就是越高 vcf

## 2.4、性能分析

### 2.4.1、MySql Query Optimizer

- MySql中专门负责优化SELECT语句的优化器模块，主要功能：通过计算分析系统中收集到的统计信息，为客户端请求的Query提供它认为最优的执行计划（它认为最优的数据检索方式，不见得是程序员认为的，这部分最耗时间）
- 当客户端向MySql请求一条Query，命令解析器模块完成请求分类，区别出是SELECT并转发给MySql Query Optimizer时，MySql Query Optimizer首先会对整条Query进行优化，处理掉一些常量表达式的预算，直接换算成常量值。并对Query中的查询条件进行简化和转换，如去掉一些无用或显而易见的条件、结构调整等。然后分析Query中的Hint信息，看显示Hint信息是否可以完全确定该Query的执行计划。如果没有Hint或Hint信息还不足以完全确定执行计划，则会读取所涉及对象的统计信息，根据Query进行写相应的计算分析，然后在得出最后的执行计划。

**MySql常见瓶颈**

- CPU：CPU在饱和的时候一般发生在数据装入内存或从磁盘上读取数据时候
- IO：磁盘I/O瓶颈发生在装入数据远大于内存容量的时候
- 服务器硬件的性能瓶颈：top、free、iostat、vmstat来查看系统的性能瓶颈

### 2.4.2、Explain

**Explain：使用Explain关键字可以模拟优化器执行SQL查询语句，从而知道MySql是如何处理SQL语句。分析查询语句或是表结构的性能瓶颈 **

- 表的读取顺序
- 数据读取操作的操作类型
- 哪些索引可以使用
- 哪些索引被实际使用
- 表之间的使用
- 每张表有多少行被优化器查询

#### 2.4.2.1、使用

**Explain +  SQL语句**

```mysql
explain select * from tb_dept;
```

![image-20200928220938859](image-20200928220938859.png)

#### 2.4.2.2、名词字段解释

##### id

select查询的序列号，包含一组数字，表示查询中执行select子句或操作表的顺序

- **id相同：执行顺序由上至下**

  ```mysql
   explain select * from tb_dept a, tb_emp b where a.id = b.deptId;
  ```

  ![image-20200928221730791](image-20200928221730791.png)

  先加载a表在加载b表，按顺序

- id不同：如果是子查询，id的序号会递增，**id值越大优先级越高，越先被执行**

  ```mysql
   explain select * from tb_emp 
   where deptId = ( 
       select id from tb_dept where id = 1 
   );
  ```

  ![image-20200928222223056](image-20200928222223056.png)

  先加载id值大的tb_dept表，在加载tb_emp表

- id相同不同，同时存在

  ```mysql
  explain select * 
  from tb_emp a,tb_dept b 
  where a.deptId = b.id 
  and  deptId = ( 
      select id from tb_dept where id = 1 
  );
  ```

  ![image-20200928222621609](image-20200928222621609.png)

  id如果相同，可以认为是一组，从上往下顺序执行

  在所有组中，id值越大，优先级越高，越先执行

  先执行tb_dept表在按顺序执行b然后a表

##### select_type

**查询的类型，主要是用于区别普通查询、联合查询、子查询等的复杂查询**

- **SIMPLE：**简单的select查询，查询中不包含子查询或者UNION

  ```mysql
   explain select * from tb_emp;
  ```

  ![image-20200928223826875](image-20200928223826875.png)

- **PRIMARY：**查询中若包含任何复杂的子部分，最外层查询则被标记为

  ```mysql
  explain select * from tb_emp where deptId = ( select id from tb_dept where id = 1 );
  ```

  ![image-20200928223931499](image-20200928223931499.png)

  PRIMARY是最后执行的

- **SUBQUERY：**在SELECT或WHERE列表中包含了子查询

  ```mysql
  explain select * from tb_emp where deptId = ( select id from tb_dept where id = 1 );
  ```

  ![image-20200928224034125](image-20200928224034125.png)

- **DERIVED：**在FROM列表中包含的子查询被标记为DERIVED（衍生），MYSQL会递归执行这些子查询，吧结果放在临时表里

- **UNION：**若第二个SELECT出现在UNION之后，则被标记为UNION；若UNION包含在FROM子句的子查询中，外层SELECT将被标记为：DERIVED

  ```mysql
  explain select * from tb_emp a left join tb_dept b on a.deptId = b.id union select * from tb_emp a right join tb_dept b on a.deptId = b.id;
  ```

  ![image-20200928231337818](image-20200928231337818.png)

- **UNION RESULT：**从UNION表获取结果的SELECT

  ```mysql
  explain select * from tb_emp a left join tb_dept b on a.deptId = b.id union select * from tb_emp a right join tb_dept b on a.deptId = b.id;
  ```

  ![image-20200928231401840](image-20200928231401840.png)

##### table

**显示这一行的数据是关于那张表的**

##### type

type显示的是**访问类型，结果值从最好到最坏依次是：**

system > const > eq_ref > ref > fulltext > ref_or_null > idnex_merge > unique_subquery > index_subquery > range > index > ALL

**常用的：system > const > eq_ref > ref  > range > index > ALL**

一般来说，得保证查询至少达到**range**级别，最好达到**ref**

- **system：**表只有一行数据（等于系统表），这是const类型的特列，平时不会出现，可以忽略不计
- **const：**表示通过索引一次就找到了，const用于比较primary key或者unique索引。因为只匹配一行数据，所以很快，如将主键置于where列表中，MySQL就能将查询转换为一个常量
- **eq_ref：**唯一性索引扫描，对于每个索引键，表中只有一条与之匹配。常见于主键或唯一索引扫描
- **ref：**非唯一性索引扫描，返回匹配某个单独值的所有行。本质上也是一种索引访问，它返回所有匹配某个单独值的行，然而，它可能会找到多个符合条件的行，所以他应该属于查找和扫描的混合体
- **range：**只检索给定范围的行，使用一个索引来选择行。key列显示使用了那个索引，一般就是在where语句中出现between、<、>、in等的查询，这种范围扫描比全表扫描要好，因为只需要开始于索引的某一点，而结束于另一点，不用扫描全部索引。
- **index：**Full Index Scan，index与ALL区别为indx类型只是遍历索引树。这通常比ALL快，因为索引文件通常比数据文件小。（虽然ALL和Index都是读全表，但Index是从索引中读取，而ALL是从硬盘中读的）
- **ALL：**Full Table Scan，将遍历全以找到匹配的行

##### possible_key

显示可能应用在这张表中的索引，一个或多个。

查询涉及到的字段上若存在索引，则该索引将被列出，**但不一定被查询实际使用**

理论上要使用的索引

##### key

实际使用的索引。如果为NULL，则没有使用索引

**查询中若使用了覆盖索引，则该索引仅出现在key列表中**

实际所使用的索引

```mysql
explain select * from tb_emp where deptId = 1 and id = 1;
```

![image-20200930095629447](image-20200930095629447.png)

理论上要使用的索引是：`PRIMARY,fk_dept_id`。实际上使用的索引是：`PRIMARY`

##### key_len

表示索引中使用的字节数，可以通过该列计算查询中使用的索引长度。在不损失精确性的情况下，长度越短越好。

key_len显示的值为索引字段的最大可能长度，**并非实际使用长度**，即key_len是根据表定义计算而得，不是通过表内检索出来

查询条件精度越高，值越大，同样的查询结果，精度越小越好

##### ref

显示索引的那一列被使用了，如果可能的话，是一个常数。哪些列或常量被用于查找索引列上的值

查询与其他表关联的字段，外键关系建立索引

##### rows

根据表统计信息及索引选用情况，大致估算出找到所需的记录所需要读取的行数

##### Extra

包含不适合在其它列中显示但十分重要的额外信息

- **Using filesort：**说明mysql会对数据使用一个外部的索引排序，而不是按照表内的索引顺序进行读取。MYSQL中无法利用索引完成的排序操作称为“文件排序”
- **Using temporary：**使用了临时表保存中间结果，MySQL在对查询结果排序时使用临时表。常见于排序`order by`和分组查询`group by`
- **Using index：**覆盖索引（Covering Index）
  - 表示相应的select操作中使用了覆盖索引（Covering Index），避免访问了表的数据行，效率不错！
  - 如果同时出现using where，表明索引被用来执行索引键值的查找；
  - 如果没有同时出现using where，表明索引用来读取数据而非执行查找动作。
  - 覆盖索引：一般说索引覆盖
    - 就是select的数据列只用从索引中就能够获得，不必读取数据行，MySQL可以利用索引返回select列表中的字段，而不必根据索引再次读取数据文件，**查询列要别所建的索引覆盖**
    - 索引是高效找到行的一个方法，但是一般数据库也能使用索引找到一列的数据，因此他不必读取整个行。毕竟索引叶子节点存储了他们索引的数据；当能通过读取索引就可以得到想要的数据，那就不需要读取行了。一个索引包含了满足查询结果的数据就叫做覆盖索引
    - 注意：
      - 如果要使用覆盖索引，一定要注意select列表中只取出需要的列，不可select *
      - 因为如果将所有字段一起做索引会导致索引文件过大，查询性能下降
- Using where：表明了使用where过滤
- Using join buffer：使用了连接缓存
- Impossible where：where子句的值总是false，不能用来获取任何元组
- select tables optimized away：在没有group by子句的情况下，基于索引优化MIN/MAX操作或者对于MyISAN存储引擎优化Count(*)操作，不必等到执行阶段在进行计算
- distinct：优化distinct操作，在找到第一匹配的元组后即停止找同样值的动作



# 3、查询截取分析

## 3.1、查询优化

## 3.2、慢查询日志

## 3.3、批量数据脚本

## 3.4、Show Profile

## 3.5、全局查询日志

# 4、MySQL锁机制

## 4.1、概述

## 4.2、表锁

## 4.3、行锁

## 4.4、页锁

# 5、主从复制