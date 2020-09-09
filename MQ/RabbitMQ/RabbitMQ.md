# 1、MQ

## 1.1、什么是MQ

**MQ（Message Queue）消息队列：**，是基础数据结构中“先进先出”的一种数据结构。一般用来解决应用解耦，异步消息，流量削锋等问题，实现高性能，高可用，可伸缩和最终一致性架构。

MQ，通过典型的`生产者`和`消费者`模型，生产者不断向消息队列中生产消息，消费者不断的从队列中获取消息。因为消息的生产和消费都是异步的，而且只关心消息的发送和接收，没有业务逻辑的侵入，轻松的实现系统间解耦。别名为`消息中间件` 通过利用高效可靠的消息传递机制进行平台无关的数据交流，并基于数据通信来进行分布式系统的集成。

## 1.2、主流的MQ

-  **ActiveMQ**
  - ActiveMQ是Apache出品，最流行的，能力强劲的开源消息总线。它是一个完全支持JMS规范的消息中间件。丰富的API，多种集群架构模式让ActiveMQ在业界成为老牌的消息中间件，在中小型企业颇受欢迎
- **Kafka**
  - Kafka是LinkedIn开源的分布式发布-订阅消息系统，目前归属于Apache顶级项目。Kafka主要特点是基于Pull的模式来处理消息消费，追求高吞吐量，一开始的目的就是用于日志收集和传输。0.8版本开始支持复制，不支持事务，对消息的重复、丢失、错误没有严格要求，适合产生大量数据的互联网服务的数据手机业务
- **RocketMQ**
  - RocketMQ是阿里开源的消息中间件，他是纯Java开发，具有高吞吐量、高可用性、适合大规模分布式系统应用的特点。RocketMQ思路起源于Kafka，单并不是Kafka的一个Copy，它对消息的可靠传输及事务性做了优化，目前在阿里集团被广泛应用于交易、充值、流计算、消息推送、日志流式处理、binglog分发等场景
- **RabbitMQ**
  - RabbitMQ是使用**Erlang**语言开发的开源消息队列系统，基于AMQP协议来实现。AMQP的主要特征是面向消息、队列、路由（包括点对点和发布/订阅）、可靠性、安全。AMQP协议更多用在企业系统内对数据一致性、稳定性和可靠性要求很高的场景，对性能和吞吐量的要求还在其次

> RabbitMQ比Kafka可靠，Kafka更适合IO高吞吐的处理，一般应用在大数据日志处理或对实时性（少量延迟）、可靠性（少量丢数据）要求稍低的场景使用，比如ELK日志收集

# 2、RabbitMQ

> 基于AMQP协议，erlang语言开发，是部署最广泛的开源消息中间件，最受欢迎的开源消息中间件之一

**官网：https://www.rabbitmq.com/**

**官方教程：https://www.rabbitmq.com/#getstarted**

**身份信息文档：https://www.rabbitmq.com/access-control.html**

**配置：https://www.rabbitmq.com/configure.html#config-location**

## 2.1、AMQP协议

**AMQP（advanced message queuing protocol）在2003年时被提出，最早用于解决金融领域不同平台之间的消息传递交互问题。顾名思义，AMQP是一种协议，更准确的说是一种binary wire-level protocol（链接协议）。这是其和JMS的本质差别，AMQP不从API层进行限定，而是直接定义网络交换的数据格式。这使得实现了AMQP的provider天然性是跨平台的。**

AMQP协议模型

 ![image-20200909155224245](image-20200909155224245.png)

## 2.2、下载安装

### 2.2.1、下载

RabbitMQ下载地址：https://www.rabbitmq.com/download.html

![image-20200909163130793](image-20200909163130793.png)

![image-20200909163205493](image-20200909163205493.png)

![image-20200909163224163](image-20200909163224163.png)

**RabbitMQ基于Erlang开发，所以先要安装Erlang环境**

查看RabbitMQ与Erlang的版本依赖关系：https://www.rabbitmq.com/which-erlang.html

![image-20200909163516921](image-20200909163516921.png)

![image-20200909164225359](image-20200909164225359.png)

**选择精简版进行安装**

![image-20200909164639827](image-20200909164639827.png)

![image-20200909164750399](image-20200909164750399.png)

![image-20200909164802480](image-20200909164802480.png)

![image-20200909164851518](image-20200909164851518.png)

### 2.2.2、安装

- 将下载的rpm包上传到服务器中

- 安装**Erlang**

  - ```bash
    rpm -ivh erlang-22.3.4.10-1.el6.x86_64.rpm 
    ```

  - 查看Erlang版本

    ```bash
    erl –version
    ```

    ![image-20200909215200431](image-20200909215200431.png)

- 安装**socat **依赖包

  ```bash
  yum install -y socat
  ```

- 安装**RabbitMQ**

  ```bash
  rpm -ivh rabbitmq-server-3.8.8-1.el7.noarch.rpm 
  ```

- 安装**RabbitMQ的Web界面插件**

  ```bash
  rabbitmq-plugins enable rabbitmq_management 
  ```

  默认端口**15672**，开启防火墙

  ```bash
  firewall-cmd --zone=public --add-port=15672/tcp --permanent   # 开启端口
  systemctl restart firewalld.service							  # 重启防火墙
  ```

- **开启**

  ```bash
  systemctl start rabbitmq-server
  ```

- **查看状态**

  ```bash
  systemctl status rabbitmq-server
  ```

  ![image-20200909220013339](image-20200909220013339.png)

- **外部访问**

  http://192.168.245.200:15672/

  ![image-20200909220118833](image-20200909220118833.png)

  ![image-20200909222435153](image-20200909222435153.png)

- **关闭**

  ```bash
  systemctl stop rabbitmq-server
  ```

### 2.2.3、常用命令

#### 2.2.3.1、管理命令行

```bash
# 1.服务启动相关
systemctl start|restart|stop|status rabbitmq-server

# 2.管理命令行 用来在不使用web管理界面的情况下命令操作RabbitMQ
rabbitmqctl help  # 可以查看更多命令

# 3.插件管理命令行
rabbitmq-plugins enable|list|disable
```

#### 2.2.3.2、管理账户

```bash
# 在rabbitmq的内部数据库添加用户；
rabbitmqctl add_user <username> <password>  
 
# 删除一个用户；
rabbitmqctl delete_user <username>  
 
# 改变用户密码（也是改变web管理登陆密码）；
rabbitmqctl change_password <username> <newpassword>  
 
# 清除用户的密码，该用户将不能使用密码登陆，但是可以通过SASL登陆如果配置了SASL认证；
rabbitmqctl clear_password <username>
 
# 设置用户tags；
rabbitmqctl set_user_tags <username> <tag> ...
 
# 列出用户；
rabbitmqctl list_users  

# 授权用户远程访问
rabbitmqctl set_permissions -p / <username> "." "." ".*" 
 
# 创建一个vhosts；
rabbitmqctl add_vhost <vhostpath>  
 
# 删除一个vhosts；
rabbitmqctl delete_vhost <vhostpath>  
 
# 列出vhosts；
rabbitmqctl list_vhosts [<vhostinfoitem> ...]  
 
# 针对一个vhosts给用户赋予相关权限；
rabbitmqctl set_permissions [-p <vhostpath>] <user> <conf> <write> <read>  
 
# 清除一个用户对vhosts的权限；
rabbitmqctl clear_permissions [-p <vhostpath>] <username>  
 
# 列出哪些用户可以访问该vhosts；
rabbitmqctl list_permissions [-p <vhostpath>]  
 
# 列出该用户的访问权限；
rabbitmqctl list_user_permissions <username>  
 
rabbitmqctl set_parameter [-p <vhostpath>] <component_name> <name> <value>
rabbitmqctl clear_parameter [-p <vhostpath>] <component_name> <key>
rabbitmqctl list_parameters [-p <vhostpath>]
```

