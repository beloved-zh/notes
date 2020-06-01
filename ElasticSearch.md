# ElasticSearch安装

**最低要求JDK1.8**

官网：https://www.elastic.co/cn/

下载地址：https://www.elastic.co/cn/downloads/elasticsearch

**下载之后解压即可**

![image-20200526110211642](http://image.beloved.ink/Typora/image-20200526110211642.png)

**目录说明**

```
bin		启动文件
config	配置文件
	log4j2				日志配置文件
	jvm.options			java虚拟机相关配置
	elasticsearch.yml	elasticsearch的配置文件	默认9200端口！ 跨域！
lib		相关jar包
logs	日志
modules	功能模块
plugins	插件 ik分词器
```

**启动。**

`bin/elasticsearch.bat`

![image-20200526111004821](http://image.beloved.ink/Typora/image-20200526111004821.png)

**访问测试**

http://127.0.0.1:9200/

![image-20200526120721138](http://image.beloved.ink/Typora/image-20200526120721138.png)

# ES head安装

下载地址：https://github.com/mobz/elasticsearch-head/tree/master

进入解压目录

```npm
npm install
npm run start
```

**访问：http://localhost:9100**

![image-20200526121453389](http://image.beloved.ink/Typora/image-20200526121453389.png)

**解决跨域问题**

修改ES配置文件，开启跨域访问

`\config\elasticsearch.yml`

```yml
# 开启跨域访问
http.cors.enabled: true
# 允许所有人访问
http.cors.allow-origin: "*"
```

重启ES

![image-20200526122022767](http://image.beloved.ink/Typora/image-20200526122022767.png)

# Kibana安装

kibana是一个针对ElasticSearch的开源分析及可视化平台，用来搜索、查看交互存储在ElasticSearch索引中的数据。使用Kibana，可以通过各种图表进行高级数据分析及展示，Kibana让海量数据更容易理解。它操作简单，基于浏览器的用户界面可以快速的创建仪表板实时显示ElasticSearch查询动态。设置Kibana非常简单。无需编码或者额外的基础架构，几分钟内就可以完成Kibana安装并启动				ElasticSearch索引监测

官网：https://www.elastic.co/cn/kibana

**注意：Kibana要与ES的版本一致**

**解压后的目录**

![image-20200526131935641](http://image.beloved.ink/Typora/image-20200526131935641.png)

**启动**

`bin\kibana.bat`

**注意：启动有点慢，需要等待**

![image-20200526132830262](http://image.beloved.ink/Typora/image-20200526132830262.png)

**访问测试： http://localhost:5601**

![image-20200526132956301](http://image.beloved.ink/Typora/image-20200526132956301.png)

**开发工具**

![image-20200526133044337](http://image.beloved.ink/Typora/image-20200526133044337.png)

**汉化：修改Kibana配置即可  zh-CN**

`\config\kibana.yml`

```yml
#i18n.locale: "en"

# 汉化
i18n.locale: "zh-CN"
```

**重启测试**

![image-20200526133634839](http://image.beloved.ink/Typora/image-20200526133634839.png)

# ES核心概念

> 概述

ElasticSearch是面向文档，关系行数据库。一切都是JSON

| Relational DB      | ElasticSearch   |
| ------------------ | --------------- |
| 数据库（database） | 索引（indices） |
| 表（tables）       | types           |
| 行（rows）         | documents       |
| 字段（columns）    | fields          |

ElasticSearch（集群）中可以包含多个索引（数据库），每个索引中可以包含多个类型（表），每个类型下又包含多个文档（行），每个文档中又包含多个字段（列）

**物理设计：**

ElasticSearch在后台吧每个**索引划分成多个分片**，每个分片可以在集群中的不同服务器间迁移

一个就是一个集群，默认名是ElasticSearch

**逻辑设计：**

一个索引类型中，包含多个文档，比如说文档1，文档2。当索引一篇文档时，可以通过这样的一个顺序找到它：索引>类型>文档id，通过这个组合就索引到具体的文档

> 文档

ElasticSearch时面向文档的，意味着索引和搜索数据的最小单位是文档，ElasticSearch中，文档几个重要属性：

- 自我包含，一篇文档同时包含字段和对应的值，也就是同时包含key:value !
- 可以是层次型的，一个文档中包含自文档，复杂的逻辑实体就是这么来的！ {就是一个json对象！ fastjson进行自动转换！}
- 灵活的结构，文档不依赖预先定义的模式，我们知道关系型数据库中，要提前定义字段才能使用，在elasticsearch中，对于字段是非常灵活的，有时候，我们可以忽略该字段，或者动态的添加一个新的字段

尽管我们可以随意的新增或者忽略某个字段，但是，每个字段的类型非常重要，比如一个年龄字段类型，可以是字符串也可以是整形，因为elasticsearch会保存字段和类型之间的映射及其他的设置.这种映射具体到每个映射的每种类型，这也是为什么在 elasticsearch中，类型有时候也称为映射类型。

> 类型

类型是文档的逻辑容器，就像关系型数据库一样，表格是行的容器。类型中对于字段的定义称为映射，比如name映射为字符串类型。我们说文档是无模式的，它们不需要拥有映射中所定义的所有字段，比如新增一个字段，那么elasticsearch是怎么做的呢？elasticsearch会自动的将新字段加入映射，但是这个字段的不确定它是什么类型，elasticsearch就开始猜，如果这个值是18 ,那么 elasticsearch会认为它是整形。但是elasticsearch也可能猜不对，所以最安全的方式就是提前定义好所需要的映射，这点跟关系 型数据库殊途同归了，先定义好字段，然后再使用，别整什么幺蛾子。

> 索引

索引是映射类型的容器，elasticsearch中的索引是一个非常大的文档集合.索引存储了映射类型的字段和其他设置#然后它们被存 
储到了各个分片上了#我们来研究下分片是如何工作的。

**物理设计：节点和分片如何工作**

一个集群至少有一个节点，而一个节点就是一个elasricsearch进程，节点可以有多个索引默认的，如果你创建索引，那么索引将会 
有个S个分片（primary shard,又称主分片）构成的，每一个主分片会有一个副本（replica shard ,又称复制分片）

![image-20200601131512087](http://image.beloved.ink/Typora/image-20200601131512087.png)

上图是一个有3个节点的集群，可以看到主分片和对应的复制分片都不会在同一个节点内，这样有利于某个节点挂掉了，数据也不至于丟失。实际上，一个分片是一个Lucene索引，一个包含==倒排索引==的文件目录.倒排索引的结构使得elasticsearch在不扫描全部文挡的情况下,就能告讶你挪些文桂包含特定的关键字。

> 倒排索引

elasticsearch使用的是一种称为倒排索引的结构，采用Lucene倒排索作为底层。这种结构适用于快速的全文搜索，一个索引由文档中所有不重复的列表构成，对于每一个词，都有一个包含它的文档列表。例如，现在有两个文档，每个文档包含如下内容：

```
Study every day, good good up to forever # 文档1包含的内容 
To forever, study every day, good good up # 文档2包含的内容
```

为了创建倒排索引，首先要将每个文档拆分成独立的词(或称为词条或者tokens),然后创建一个包含所有不重复的词条的排序 
列表，然后列出每个词条出现在哪个文档：

| term    | doc_1 | doc_2 |
| ------- | ----- | ----- |
| Study   | √     | ×     |
| To      | ×     | ×     |
| every   | √     | √     |
| forever | √     | √     |
| day     | √     | √     |
| study   | ×     | √     |
| good    | √     | √     |
| every   | √     | √     |
| to      | √     | ×     |
| up      | √     | √     |

现在，搜索to forever，只需要查看包含每个词条的文档score

| term    | doc_1 | doc_2 |
| ------- | ----- | ----- |
| to      | √     | ×     |
| forever | √     | √     |
| total   | 2     | 1     |

两个文档都匹配，但是第一个文档比第二个匹配程度更高。如果没有别的条件，现在，这两个包含关键字的文档都将返回。 

再来看_个示例，比如我们通过博客标签来搜索博客文章。那么倒排索引列表就是这样的一个结构：

![image-20200601132934746](http://image.beloved.ink/Typora/image-20200601132934746.png)

如果要搜索含有python标签的文章，对于查找所有原始数据而言，查找倒排索引后的数据将会快的多。只需要査看标签这一 栏，然后获取相关的文章ID即可。

elasticsearch的索引和Lucene的索引对比

在elasticsearch中，索引这个词被频繁使用，这就是术语的使用。在elasticsearch中，索引被分为多个分片，每份分片是一个 
Lucene的索引。==所以一个elasticsearch索引是由多个Lucene索引组成的。==别问为什么，谁让elasticsearch使用Lucene作为底层呢! 
如无特指，说起索引都是指elasticsearch的索引。 

# IK分词器插件

> 什 么 是IK分词器？ 

分词：即把一段中文或者別的划分成一个个的关键字，我们在搜索时候会把自己的信息进行分词，会把数据库中或者索引库中的数 
据进行分词，然后进行一个匹配操作，默认的中文分词是将每个字看成一个词，比如"我爱中国"会被分为”我“，”爱“，”中”，“国“，这显 
然是不符合要求的，所以我们需要安装中文分词器ik来解决这个问题。

IK提供了两个分词算法：ik_smart和ik_max_word ,其中ik_smart为最少切分，ik_max_word为最细粒度划分！

> 安装

1. 下载：https://github.com/medcl/elasticsearch-analysis-ik/releases **注意对应版本**

2. 将文件解压在ElasticSearch的插件目录下`plugins`

   ![image-20200601134255693](http://image.beloved.ink/Typora/image-20200601134255693.png)

3. 重启观察ES。可以看到IK分词器被加载了

   ![image-20200601134525762](http://image.beloved.ink/Typora/image-20200601134525762.png)

4. 使用`elasticsearch-plugin list`查看加载的插件

   ![image-20200601134824391](http://image.beloved.ink/Typora/image-20200601134824391.png)

5. 使用kibana测试

> 查看不同的分词效果

ik_smart最少切分

![image-20200601135623665](http://image.beloved.ink/Typora/image-20200601135623665.png)

ik_max_word最细粒度划分！穷尽词库！字典

![image-20200601135848316](http://image.beloved.ink/Typora/image-20200601135848316.png)

> 输入：超级喜欢乔欣

![image-20200601140246341](http://image.beloved.ink/Typora/image-20200601140246341.png)

**问题：乔欣被拆开了**

这种自己需要的词，需要手动配置到ik分词器的字典中

> IK分词器增加自己的配置

新建词典`my.dic`输入需要配置的词

```dic
张恒恒
乔欣
```

导入`my.dic`到`IKAnalyzer.cfg.xml`中

![image-20200601140754090](http://image.beloved.ink/Typora/image-20200601140754090.png)

重启测试

启动日志：加载了`my.dic`

![image-20200601141057073](http://image.beloved.ink/Typora/image-20200601141057073.png)

![image-20200601141117603](http://image.beloved.ink/Typora/image-20200601141117603.png)

# Rest风格说明

一种软件架构风格。而不是标准，只是提供了一组设计原则和约束条件。它主要用于客户端和服务端交互类的软件 。基于这个风格设计的软件可以更简介，更有层次，更易于实现缓存机制。

基本Rest命令说明：

| method |                     url地址                     |          描述          |
| :----: | :---------------------------------------------: | :--------------------: |
|  PUT   |     localhost:9200/索引名称/类型名称/文档id     | 创建文档（指定文档id） |
|  POST  |        localhost:9200/索引名称/类型名称         | 创建文档（随机文档id） |
|  POST  | localhost:9200/索引名称/类型名称/文档id/_update |        修改文档        |
| DELETE |     localhost:9200/索引名称/类型名称/文档id     |        删除文档        |
|  GET   |     localhost:9200/索引名称/类型名称/文档id     |   查询文档通过文档id   |
|  POST  |    localhost:9200/索引名称/类型名称/_search     |      查询所有数据      |

# 关于索引的基本操作

> 基础测试

1. 创建一个索引

   ![image-20200601142812112](http://image.beloved.ink/Typora/image-20200601142812112.png)

   ![image-20200601142832758](http://image.beloved.ink/Typora/image-20200601142832758.png)

2. 指定字段的类型

   ![image-20200601143554184](http://image.beloved.ink/Typora/image-20200601143554184.png)

3. 获得具体的规则信息

   ![image-20200601143716149](http://image.beloved.ink/Typora/image-20200601143716149.png)

4. 查看默认信息

   ![image-20200601144628760](http://image.beloved.ink/Typora/image-20200601144628760.png)

   ![image-20200601144754517](http://image.beloved.ink/Typora/image-20200601144754517.png)

> 修改

1. 使用PUT进行覆盖

   **必须带上所有字段。否则就会没有**

   ![image-20200601145247056](http://image.beloved.ink/Typora/image-20200601145247056.png)

2. POST

   **只需要设置需要修改的字段即可**

   ![image-20200601145649182](http://image.beloved.ink/Typora/image-20200601145649182.png)

> 删除

![image-20200601150030492](http://image.beloved.ink/Typora/image-20200601150030492.png)

# 关于文档的基本操作（重点）

> 基本操作

1. 添加数据 PUT

   ```json
   PUT test/user/1
   {
     "name": "张恒",
     "age": 20,
     "desc": "热爱技术",
     "tags": ["技术宅","直男","听歌"]
   }
   ```

   ![image-20200601150739042](http://image.beloved.ink/Typora/image-20200601150739042.png)

2. 获取数据 GET

   ![image-20200601151157418](http://image.beloved.ink/Typora/image-20200601151157418.png)

3. 更新数据   POST _update

   ![image-20200601151417721](http://image.beloved.ink/Typora/image-20200601151417721.png)

4. 简单的搜索

   根据默认的映射规则产生基本的查询

   ![image-20200601151916041](http://image.beloved.ink/Typora/image-20200601151916041.png)

> 复杂操作

![image-20200601152450086](http://image.beloved.ink/Typora/image-20200601152450086.png)

1. 指定查询的字段

   ![image-20200601152645381](http://image.beloved.ink/Typora/image-20200601152645381.png)

2. 排序

   ![image-20200601153033844](http://image.beloved.ink/Typora/image-20200601153033844.png)

3. 分页

   ![image-20200601153313437](http://image.beloved.ink/Typora/image-20200601153313437.png)

4. 布尔值查询

   `must`所有条件都要符合，相当于sql的`and`

   ![image-20200601153708575](http://image.beloved.ink/Typora/image-20200601153708575.png)

   `should`所有条件都要符合，相当于sql的`or`

   ![image-20200601154002787](http://image.beloved.ink/Typora/image-20200601154002787.png)

   `must_not`所有条件都要符合，相当于sql的`not`

   ![image-20200601154138279](http://image.beloved.ink/Typora/image-20200601154138279.png)

5. 过滤器（filter）

   `gt`：大于		`gte`：大于等于		`lt`：小于		`lte`：小于等于 		.....

   ![image-20200601154701038](http://image.beloved.ink/Typora/image-20200601154701038.png)

6. 匹配多个条件

   ![image-20200601155004884](http://image.beloved.ink/Typora/image-20200601155004884.png)

7. 精确查询

   term查询是直接通过倒排索引指定的词条进程精确查找的

   **关于分词：**

   - term：直接查询精确的
   - match：会使用分词器解析（先解析文档，然后在通过分析的文档进行查询）

   **两个类型 text 	keyword**

   text：可以被拆分

   keyword：字段类型不会被分词器解析

8. 高亮查询

   ![image-20200601155954951](http://image.beloved.ink/Typora/image-20200601155954951.png)

   **自定义标签**

   ![image-20200601160217197](http://image.beloved.ink/Typora/image-20200601160217197.png)

9. 