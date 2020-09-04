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

# 5、Zookeeper

- Zookeeper 分布式协调组件.
  - 本质一个软件.
- Zookeeper常用功能
  - 发布订阅功能.把zookeeper当作注册中心原因.
  - 分布式/集群管理功能.
- 使用java语言编写的

## 5.1、下载安装

Zookeeper官网：https://zookeeper.apache.org/index.html

3.6.1下载地址：https://www.apache.org/dyn/closer.lua/zookeeper/zookeeper-3.6.1/apache-zookeeper-3.6.1-bin.tar.gz

**Zookeeper安装前提配置好了JDK**

- 上传zookeeper 安装包到linux中/usr/local/temp 中(目录随意,对安装无影响)

- 解压zookeeper压缩包

  ```bash
  tar zxvf apache-zookeeper-3.6.1-bin.tar.gz
  ```

- 移动zookeeper解压后的文件夹到/usr/local下并起名为zookpper(复制后名称任意,对安装无影响)

  ```bash
  mv apache-zookeeper-3.6.1-bin ../zookeeper
  ```

-  进入到zookeeper文件夹中

  ```bash
  [root@localhost local]# pwd
  /usr/local
  [root@localhost local]# cd zookeeper/
  ```

- 在zookeeper中新建data文件夹,做为zookeeper数据存储文件夹

  ```bash
  mkdir data
  ```

- 进入到conf文件夹

  ```bash
  cd conf
  ```

- 复制zoo_sample.cfg,并给新起名的zoo.cfg

  ```bash
  ls
  configuration.xsl  log4j.properties  zoo_sample.cfg
  cp zoo_sample.cfg zoo.cfg  # 复制
  ls
  configuration.xsl  log4j.properties  zoo.cfg  zoo_sample.cfg
  ```

- 修改zoo.cfg中dataDir属性值为新建data文件夹的路径

  ```bash
  vim zoo.cfg
  ```

  修改后的效果   **2181为Zookeeper的端口号**

  ![image-20200904232619987](image-20200904232619987.png)

- 进入到zookeeper/bin文件夹,使用zkServer.sh启动zookeeper

  ```bash
  [root@localhost /]# cd usr/local/zookeeper/bin/  
  [root@localhost bin]# ls
  README.txt    zkCli.cmd  zkEnv.cmd  zkServer.cmd            zkServer.sh            zkSnapShotToolkit.sh  zkTxnLogToolkit.sh
  zkCleanup.sh  zkCli.sh   zkEnv.sh   zkServer-initialize.sh  zkSnapShotToolkit.cmd  zkTxnLogToolkit.cmd
  [root@localhost bin]# ./zkServer.sh 执行命令 查看参数
  ZooKeeper JMX enabled by default
  Using config: /usr/local/zookeeper/bin/../conf/zoo.cfg
  Usage: ./zkServer.sh [--config <conf-dir>] {start|start-foreground|stop|version|restart|status|print-cmd}
  [root@localhost bin]# ./zkServer.sh start  # 启动
  ZooKeeper JMX enabled by default
  Using config: /usr/local/zookeeper/bin/../conf/zoo.cfg
  Starting zookeeper ... STARTED
  ```

  启动效果图

  ![image-20200904233146844](image-20200904233146844.png)

-  查看状态,其中Mode: standalone表示单机版

  ```bash
  ./zkServer.sh status
  ```

  ![image-20200904233255846](image-20200904233255846.png)

- 为了外部能访问,需要在防火墙中放行2181端口

  **这里使用默认防火墙firewalle不是iptables**

  - 查看Zookeeper默认端口2181是否开启

    ```bash
    firewall-cmd --query-port=2181/tcp
    ```

  - 开启2181端口

    ```bash
    firewall-cmd --zone=public --add-port=2181/tcp --permanent
    ```

  - 重启防火墙

    ```bash
    firewall-cmd --reload
    ```

  - 再次查询是否开启

  - 查询哪些端口是开启的

    ```bash
    firewall-cmd --list-port
    ```

  **具体防火墙命令可参考：**https://www.linuxidc.com/Linux/2019-06/159104.htm