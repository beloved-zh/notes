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