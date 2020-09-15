# 1、微服务概述

## 1.1、微服务

**百度百科：**https://baike.baidu.com/item/%E5%BE%AE%E6%9C%8D%E5%8A%A1/18758759?fr=aladdin

**微服务（Microservice Architecture）：**一种软件开发技术- 面向服务的体系结构（SOA）架构样式的一种变体，将应用程序构造为一组松散耦合的服务。在微服务体系结构中，服务是细粒度的，协议是轻量级的。

微服务（或微服务架构）是一种云原生架构方法，其中单个应用程序由许多松散耦合且可独立部署的较小组件或服务组成。这些服务通常

- 有自己的堆栈，包括数据库和数据模型；
- 通过REST API，事件流和消息代理的组合相互通信；
- 和它们是按业务能力组织的，分隔服务的线通常称为有界上下文。

尽管有关微服务的许多讨论都围绕体系结构定义和特征展开，但它们的价值可以通过相当简单的业务和组织收益更普遍地理解：

- 可以更轻松地更新代码。
- 团队可以为不同的组件使用不同的堆栈。
- 组件可以彼此独立地进行缩放，从而减少了因必须缩放整个应用程序而产生的
- 浪费和成本，因为单个功能可能面临过多的负载。

微服务化的核心就是将传统的一站式应用，根据业务拆分成一个一个的服务，彻底的去耦合，每一个微服务提供单个业务功能的服务，一个服务就做一件事，从技术角度看就是一种小而独立的处理过程，类似进程的概念，能够自行单独启动或销毁，拥有自己独立的数据库。

## 1.2、微服务与微服务架构

**微服务**

强调的是服务大小，他关注的是一个点，是具体解决某一个问题/提供落地对应服务的一个服务应用，狭义的看，可以当作是IDEA中的一个个微服务工程或者Moudel

**微服务架构**

一种新的架构形式，Matrin Fowler 2014提出

**原文：**https://martinfowler.com/articles/microservices.html

**汉化：**https://www.cnblogs.com/liuning8023/p/4493156.html

微服务架构是一种架构模式，他提倡将单一应用程序划分成一组小的服务，服务之间互相协调，互相配合，为用户提供最终价值。每个服务运行在其独立的进程，服务于服务之间采用轻量级的通信机制互相协作，每个服务都围绕者具体的业务进行构建，并且能够被独立的部署到生产环境中，另外，应用尽量避免统一的，集中式的服务管理机制，对具体的一个服务而言，应根据业务上下文，选择合适的语言，工具对其进行构建（服务之间可以使用不同的语言，只要进行统一的通信机制）

## 1.3、微服务优缺点

**优点：**

- 单一职责原则
- 每个服务足够内聚，足够小，代码容易理解，这样能聚焦一个指定的业务功能或业务需求
- 开发简单，开发效率高，一个服务可能就是单一只干一件事
- 微服务是松耦合的，是有功能意义的服务，无论是在开发阶段或者部署阶段都是独立的
- 微服务可以使用不同语言开发
- 易于和第三方集成，微服务允许容易且灵活的方式集成自动部署，通过持续集成工具
- 微服务已于理解、修改和维护
- 微服务允许利用融合最新技术
- **微服务只是业务逻辑代码，不和HTML，CSS或者其他界面融合**
- **每个微服务都有自己的存储能力，可以有自己的数据库，也可以有统一数据库**
- ......

**缺点：**

- 开发人员要处理分布式系统的复杂性
- 多服务运维艰难，随着服务的增加，运维的压力也在增大
- 系统部署依赖
- 服务间通信成本
- 数据一致性
- 系统集成测试
- 性能监控
- ......

# 2、SpringCloud

## 2.1、SpringCloud

**官网：https://spring.io/projects/spring-cloud**

**微服务架构图**

![image-20200915213251203](image-20200915213251203.png)

**SpringCloud**：基于SpringBoot提供了一套微服务解决方案，包括服务注册与发现、配置中心、全链路监控、服务网关、负载均衡、熔断器等组件，除了基于NetFlix的开源组件做高度抽象封装之外，还有一些选择型中立的开源组件。

SpringCloud利用SpringBoot的开发便利性，巧妙的简化了分布式系统基础设施的开发，SpringCloud为开发人员提供了快速构建分布式的一些工具，**包括配置管理、服务发现、断路器、路由、微代理、事件总线、全局锁、决策竞选、分布式会话等等，**他们都可以使用SpringBoot的开发风格做到一键启动和部署

SpringBoot并没有重复造轮子，他只是将目前各家公司开发的比较成熟，经得起实际考研的服务框架组合起来，通过SpringBoot风格进行再次封装，屏蔽掉了复杂的配置和实现原理，**最终给开发者留出了一套简单易懂，易部署和易维护的分布式系统开发工具包**

SpringCloud是分布式微服务框架下的一站式解决方案，是各个微服务框架落地技术的集合体，俗称微服务全家桶

##  2.2、SpringCloud和SpringBoot

- SpringBoot专注于快速方便的开发单个个体微服务
- SpringCloud是关注全局微服务协调整理治理框架，他将SpringBoot开发的一个个单体微服务整合并管理起来，为各个微服务之间提供：配置管理、服务发现、断路器、路由、微代理、事件总线、全局锁、决策竞选、分布式会话等等集成服务
- SpringBoot可以离开SpringCloud独立使用，开发项目，但是SpringCloud离不开SpringBoot，属于依赖关系
- **SpringBoot专注于快熟、方便的开发单个个体微服务，SpringCloud关注全局的服务治理框架**

## 2.3、Dubbo和SpringCloud技术选型

|              | Dubbo         | Spring                        |
| :----------- | :------------ | ----------------------------- |
| 服务注册中心 | Zookeeper     | Spring Cloud Netfilx Eureka   |
| 服务调用方式 | RPC           | REST API                      |
| 服务监控     | Dubbo-monitor | Spring Boot Admin             |
| 断路器       | 不完善        | Spring Cloud Netfilx  Hystrix |
| 服务网关     | 无            | Spring Cloud Netfilx Zuul     |
| 分布式配置   | 无            | Spring Cloud Config           |
| 服务跟踪     | 无            | Spring Cloud Sleuth           |
| 消息总线     | 无            | Spring Cloud Bus              |
| 数据流       | 无            | Spring Cloud Stream           |
| 批量任务     | 无            | Spring Cloud Task             |

**最大区别：SpringCloud抛弃了Dubbo的RPC通信，采用地是基于HTTP的REST方式**

HTTP相对于RPC来说牺牲了服务调用的性能，但也避免了上面提到的原生RPC带来的问题，而且REST相比RPC更为灵活，服务提供方和调用方的依赖只依靠一纸契约，不存在代码级别的强依赖

**SpringCloud相对于Dubbo的生态更为完全**

**Dubbo的定位是一款RPC框架**

**Spring Cloud的目标是微服务架构下的一站式解决方案**

## 2.4、版本说明

**参考文档：https://blog.csdn.net/chen497147884/article/details/79896141**

![image-20200915221616947](image-20200915221616947.png)

Spring Cloud是一个由众多独立子项目组成的大型综合项目，每个子项目有不同的发行节奏，都维护着自己的发布版本号。Spring Cloud通过一个资源清单BOM（Bill of Materials）来管理每个版本的子项目清单。为避免与子项目的发布号混淆，所以没有采用版本号的方式，而是通过命名的方式。

这些版本名称的命名方式采用了伦敦地铁站的名称，同时根据字母表的顺序来对应版本时间顺序，比如：最早的Release版本：Angel，第二个Release版本：Brixton，然后是Camden、Dalston、Edgware，目前最新的是Finchley版本。

当一个版本的Spring Cloud项目的发布内容积累到临界点或者解决了一个严重bug后，就会发布一个“service releases”版本，简称SRX版本，其中X是一个递增数字。当前官网上最新的稳定版本是Edgware.SR3，里程碑版本是Finchley.M9。下表列出了这两个版本所包含的子项目及各子项目的版本号

**SpringCloud与SpringBoot版本依赖**

![image-20200915221856649](image-20200915221856649.png)

## 2.5、参考文档

**Spring Cloud Netflix中文文档：**https://www.springcloud.cc/spring-cloud-netflix.html

**中文API文档：**https://www.springcloud.cc/spring-cloud-dalston.html

**SpringCloud中国社区：**https://springcloud.cn/

**SpringCloud中文网：**https://www.springcloud.cc/

# 4、基础环境搭建

**注意：所有的工程都是maven项目**

## 4.1、创建父工程

创建`SpringCloud`父工程

定义`pom`文件，统一管理jar包

**打包方式选择pom**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.zh</groupId>
    <artifactId>SpringCloud</artifactId>
    <version>1.0-SNAPSHOT</version>

    <!--打包方式-->
    <packaging>pom</packaging>

    <!--版本-->
    <properties>
        <mysql.version>8.0.21</mysql.version>
        <druid.version>1.1.21</druid.version>
        <mybatis.version>2.1.2</mybatis.version>
        <logback.version>1.2.3</logback.version>
        <junit.version>4.13</junit.version>
        <log4j.version>1.2.17</log4j.version>
        <lombok.version>1.18.12</lombok.version>
    </properties>

    <!--管理依赖，并不引入子项目-->
    <dependencyManagement>
        <dependencies>
            <!-- SpringCloud依赖 -->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR8</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--SpringBoot-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.3.3.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--数据库-->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>${druid.version}</version>
            </dependency>
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>${mybatis.version}</version>
            </dependency>
            <!--日志测试-->
            <dependency>
                <groupId>ch.qos.logback</groupId>
                <artifactId>logback-core</artifactId>
                <version>${logback.version}</version>
            </dependency>
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${junit.version}</version>
            </dependency>
            <dependency>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
                <version>${log4j.version}</version>
            </dependency>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>${lombok.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```

## 4.2、创建数据库

创建`db01`数据库

```sql
create table dept(
    id int(11) primary key auto_incerment,
    dname varchar(60),
    db_source varchar(60)
);

-- DATABASE()：获取当前数据库名
insert into dept(dname,db_source) values('开发部',DATABASE());
insert into dept(dname,db_source) values('人事部',DATABASE());
insert into dept(dname,db_source) values('财务部',DATABASE());
insert into dept(dname,db_source) values('市场部',DATABASE());
insert into dept(dname,db_source) values('运维部',DATABASE());

select * from dept
```

## 4.3、创建SpringCloud-api模块

**用于存放公共的pojo，server....**

![image-20200915225313146](image-20200915225313146.png)

### 4.3.1、pom

导入所需`jar`包

```xml
<!--当前的Module自己需要的依赖，如果父依赖已经配置了版本，这里不需要写版本-->
<dependencies>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </dependency>
</dependencies>
```

### 4.3.2、创建实体类

**注意：微服务各个服务之间通信，实体类需要实例化**

```java
package com.zh.pojo;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import java.io.Serializable;

/**
 * @author Beloved
 * @date 2020/9/13 17:01
 */
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Dept implements Serializable {

    private Long id;
    private String dname;

    // 标识数据库 有可能一个服务对应一个数据库 同一信息在不同数据库
    private String db_source;

}
```

## 4.4、创建服务提供者

**创建`SpringCloud-provider-dept-8000`模块**

### 4.4.1、pom

```xml
<!--引入自定义的模块，我们就可以使用这个模块中的类了-->
<dependency>
    <groupId>com.zh</groupId>
    <artifactId>SpringCloud-api</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>

<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
</dependency>

<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>

<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
</dependency>

<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-core</artifactId>
</dependency>

<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-test</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```

### 4.4.2、application.yml

```yaml
server:
  port: 8000

# mybatis配置
mybatis:
  type-aliases-package: com.zh.pojo
  mapper-locations: classpath:mybatis/mapper/*.xml

# spring配置
spring:
  application:
    name: SpringCloud-provider-dept
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db01?useUnicode=true&characterEncoding=utf-8
    username: root
    password: 123456
```

### 4.4.3、Mapper接口

```java
package com.zh.mapper;

import com.zh.pojo.Dept;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

import java.util.List;

@Mapper
@Repository
public interface DeptMapper {

    Dept findById(Long id);

    List<Dept> findAll();

}
```

### 4.4.4、Mapper.xml

**文件路径：**`mybatis/mapper/DeptMapper.xml`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.zh.mapper.DeptMapper">

    <select id="findById" resultType="Dept" parameterType="Long">
        select *
        from db01.dept
        where id = #{id}
    </select>

    <select id="findAll" resultType="Dept">
        select * from db01.dept;
    </select>
    
</mapper>
```

### 4.4.5、Service服务层接口

```java
package com.zh.service;

import com.zh.pojo.Dept;

import java.util.List;

public interface DeptService {

    Dept findById(Long id);

    List<Dept> findAll();

}
```

### 4.4.6、Service接口实现类

```java
package com.zh.service.impl;

import com.zh.mapper.DeptMapper;
import com.zh.pojo.Dept;
import com.zh.service.DeptService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

/**
 * @author Beloved
 * @date 2020/9/13 17:26
 */
@Service
public class DeptServiceImpl implements DeptService {

    @Autowired
    private DeptMapper deptMapper;

    public Dept findById(Long id) {
        return deptMapper.findById(id);
    }

    public List<Dept> findAll() {
        return deptMapper.findAll();
    }
}
```

### 4.4.7、Controller

**使用Rest服务，SpringCloud使用的通信机制基于Rest**

```java
/**
 * @author Beloved
 * @date 2020/9/13 17:29
 */
@RestController
public class DeptController {

    @Autowired
    private DeptService deptService;

    @GetMapping("/dept/get/{id}")
    public Dept findById(@PathVariable("id") Long id){
        return deptService.findById(id);
    }

    @GetMapping("/dept/list")
    public List<Dept> findAll(){
        return deptService.findAll();
    }

}
```

### 4.4.8、主启动类

```java
/**
 * @author Beloved
 * @date 2020/9/13 17:33
 */
@SpringBootApplication
public class DeptProvider_8000 {

    public static void main(String[] args) {
        SpringApplication.run(DeptProvider_8000.class,args);
    }
}
```

### 4.4.9、启动测试

![image-20200915231732703](image-20200915231732703.png)

## 4.5、创建服务消费者

**创建`SpringCloud-consumer-dept-8080`模块**

**注意：消费者是没有server接口，消费者只是提供最终接口**

### 4.5.1、pom

```xml
<dependency>
    <groupId>com.zh</groupId>
    <artifactId>SpringCloud-api</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```

### 4.5.2、application.yml

```yaml
server:
  port: 8080
```

### 4.5.3、RestTemplate

**参考文章：https://www.cnblogs.com/javazhiyin/p/9851775.html**

**RestTemplate**提供了多种便捷访问远程Http服务的方法，是一种简单便捷的访问restful服务模板 类，是Spring提供的用于访问Rest服务的客户端模板工具集

**配置RestTemplate**

```java
/**
 * @author Beloved
 * @date 2020/9/13 17:37
 */
@Configuration
public class ConfigBean {

    @Bean
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }

}
```

### 4.5.4、Controller

```java
/**
 * @author Beloved
 * @date 2020/9/13 17:39
 */
@RestController
public class DeptConsumerController {

    // 消费者，直接调用生产者

    @Autowired
    private RestTemplate restTemplate; // 提供多种便携访问http服务的方法

    // 配置生产者地址
    private static final String REST_URL_PREFIX = "http://localhost:8000";

    @GetMapping("/consumer/dept/list")
    public List<Dept> list(){
        // 通过RestTemplate使用restful风格调用生产者接口
        return restTemplate.getForObject(REST_URL_PREFIX+"/dept/list/",List.class);
    }

    @GetMapping("/consumer/dept/get/{id}")
    public Dept get(@PathVariable("id") Long id){
        return restTemplate.getForObject(REST_URL_PREFIX+"/dept/get/"+id,Dept.class);
    }
}
```

### 4.5.5、主启动类

```java
/**
 * @author Beloved
 * @date 2020/9/13 17:50
 */
@SpringBootApplication
public class DeptConsumer_8080 {

    public static void main(String[] args) {
        SpringApplication.run(DeptConsumer_8080.class,args);
    }

}
```

### 4.5.6、测试

![image-20200915233648887](image-20200915233648887.png)

# 5、Eureka

## 5.1、原理讲解

Eureka是Netfix的一个子模块，也是核心模块之一。Eureka是一个基于REST的服务，用于定位服务，以实现云端中间层服务发现和故障转移，服务注册与发现对于微服务来说非常重要的，有了服务发现与注册，只需要使用服务的标识符，就可以访问到服务，而不需要修改服务调用的配置文件，类似于Zookeeper。

**Eureka的基本架构：**

Eureka采用了C-S的架构设计，EurekaServer作为服务注册功能的服务器，他是服务注册中心

系统中的其他微服务，使用Eureka的客户端连接到EurekaServer并维持心跳连接。可以通过EurekaServer来监控系统中各个微服务是否正常运行，SpringCloud的一些其他模块（比如Zull）就可以通过EurekaServer来发现系统中的其他微服务，并执行相关的逻辑

**Eureka与Dubbo架构对比图：**

![image-20200916000007069](image-20200916000007069.png)

EurekaServer提供服务注册服务，各个节点启动后，会在EurekaServer中进行注册，这样EurekaServer中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观的看到。

EurekaClient是一个Java客户端，用于简化EurekaServer的交互，客户端同时也具备一个内置的，使用伦循负载算法的负载均衡器。在应用启动后，将会向EurekaServer发送心跳（默认周期为30秒）。如果EurekaServer在多个心跳周期内没有接收到某个节点的心跳，EurekaServer将会从服务注册表中吧这个服务节点移除调（默认周期为90秒）

**三大角色：**

- **Eureka Server**：提供服务的注册与发现
- **Server Provider**：将自身注册到Eureka中，从而使消费方能够找到
- **Server Consumer**：服务消费方从Eureka获取注册服务列表，从而找到消费者

