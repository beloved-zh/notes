# 1、SOA

**SOA：面向服务架构（Service-Oriented Architecture）**

## 1.1、SOA定位：

- 如何**设计项目**，让开发时更有效率
- SOA是一种思想

## 1.2、之前项目设计架构

- 在公司项目不允许所哟项目都能访问数据库
- 开发时，数据库访问层代码可能出现冗余

## 1.3、SOA架构

- 有一个专门提供服务的单元，专门访问数据库的服务（项目）
- 其他所有单元都调用这个服务
- 开发时可以实现，数据访问控制和代码复用

## 1.4、实现SOA架构常用服务

- Doubbo
- WebService
- DoubboX
- 服务方就是web项目，调用web项目的控制器
  - 使用HttpClient可以调用其他项目的控制器

# 2、RPC

**RPC：远程过程调用协议（Remote Procedure Call）**

## 2.1、RPC解析

客户端(A)通过互联网调用远程服务器,不知道远程服务器具体实现,只知道远程服务器提供了什么功能.

![image-20200904174610678](image-20200904174610678.png)

## 2.2、RPC优点

- 数据安全性

# 3、Dubbo

**Dubbo:一个分布式、高性能、透明化的RPC服务框架**

## 3.1、作用

**提供服务自动注册、自动发现等高效服务治理方案.**

## 3.2、Dubbo架构图

- **Provider** ：提供者,服务发布方.

- **Consumer**：消费者, 调用服务方

- **Container**：Dubbo容器.依赖于Spring容器.

- **Registry**：注册中心.当Container启动时把所有可以提供的服务列表上Registry中进行注册.
  - 作用：告诉Consumer提供了什么服务和服务方在哪里.

- Monitor：监听器

- 虚线都是异步访问,实线都是同步访问

- 蓝色虚线:在启动时完成的功能

- 绿色虚线(实线)都是程序运行过程中执行的功能

- 所有的角色都是可以在单独的服务器上.所以必须遵守特定的协议.

![image-20200413122859701](image-20200413122859701.png)

## 3.3、运行原理

- **0（start）**：启动容器,相当于在启动Dubbo的Provider

- **1（register）**：启动后会去注册中心进行注册.注册所有可以提供的服务列表

- **2（subscribe）**：在Consumer启动后会去Registry中获取服务列表和Provider的地址.进行订阅.
- **3（notify）**：当Provider有修改后,注册中心会把消息推送给Consummer
  - 使用了观察者设计模式(又叫发布/订阅设计模式)

- **4（invoke）**：根据获取到的Provider地址,真实调用Provider中功能.
  - 在Consumer方使用了代理设计模式.创建一个Provider方类的一个代理对象.通过代理对象获取Provider中真实功能,起到保护Provider真实功能的作用.

- **5（count）**Consumer和Provider每隔1分钟向Monitor发送统计信息,统计信息包含,访问次数,频率等.

# 4、Dubbo支持的注册中心

- Zookeeper
  - 优点：支持网络集群
  - 缺点：稳定性受限于Zookeeper

- Redis 
  - 优点：性能高.
  - 缺点：对服务器环境要求较高.

- Multicast
  - 优点：面中心化,不需要额外安装软件.
  - 缺点：建议同机房(局域网)内使用

- Simple
  - 适用于测试环境.不支持集群.

# 5、Zookeeper讲解

- Zookeeper 分布式协调组件.
  - 本质一个软件.

- Zookeeper常用功能
  - 发布订阅功能.把zookeeper当作注册中心原因.
  - 分布式/集群管理功能.

- 使用java语言编写的.