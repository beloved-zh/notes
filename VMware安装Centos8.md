# 下载

CentOs阿里镜像下载地址：**http://mirrors.aliyun.com/centos/

![image-20200508122007299](http://image.beloved.ink/Typora/image-20200508122007299.png)

# 安装

VMware创建新的虚拟机，选择自定义安装

![image-20200508122049495](http://image.beloved.ink/Typora/image-20200508122049495.png)

![image-20200508122128477](http://image.beloved.ink/Typora/image-20200508122128477.png)

选择稍后安装系统

![image-20200508122154913](http://image.beloved.ink/Typora/image-20200508122154913.png)

**发现没有Centos8的选项，版本选择Red Hat Entperprise Linux 8**

![image-20200508122743741](http://image.beloved.ink/Typora/image-20200508122743741.png)

给虚拟机起个名字并选择安装的位置

![image-20200508122941373](http://image.beloved.ink/Typora/image-20200508122941373.png)

根据宿主机的配置选择虚拟机的配置。按需配置

![image-20200508123109198](http://image.beloved.ink/Typora/image-20200508123109198.png)

![image-20200508123124294](http://image.beloved.ink/Typora/image-20200508123124294.png)

网络类型选择桥接网络

![image-20200508123221903](http://image.beloved.ink/Typora/image-20200508123221903.png)

选择推荐类型即可

![image-20200508123233696](http://image.beloved.ink/Typora/image-20200508123233696.png)

![image-20200508123244452](http://image.beloved.ink/Typora/image-20200508123244452.png)

磁盘创建新的磁盘

![image-20200508131323413](http://image.beloved.ink/Typora/image-20200508131323413.png)

根据自己的需求选择磁盘的容量

![image-20200508131429333](http://image.beloved.ink/Typora/image-20200508131429333.png)

磁盘名称默认即可

![image-20200508131511592](http://image.beloved.ink/Typora/image-20200508131511592.png)

选择自定义硬件

![image-20200508131553398](http://image.beloved.ink/Typora/image-20200508131553398.png)

选择自己的镜像文件，其余设备按需配置

![image-20200508131729448](http://image.beloved.ink/Typora/image-20200508131729448.png)

启动虚拟机

![image-20200508131836489](http://image.beloved.ink/Typora/image-20200508131836489.png)

开启虚拟机后会出现以下界面。选择第一项

![image-20200508132150589](http://image.beloved.ink/Typora/image-20200508132150589.png)

选择安装过程的语言，这里选择中文

![image-20200508132332603](http://image.beloved.ink/Typora/image-20200508132332603.png)

所以有**感叹号的都需要配置**，点击进入

![image-20200508133012910](http://image.beloved.ink/Typora/image-20200508133012910.png)

选择自定义，点击完成

![image-20200508133046057](http://image.beloved.ink/Typora/image-20200508133046057.png)

点击红框中的

![image-20200508133156562](http://image.beloved.ink/Typora/image-20200508133156562.png)

使用默认配置就可以，点击完成

![image-20200508133242081](http://image.beloved.ink/Typora/image-20200508133242081.png)

接收更改

![image-20200508133311445](http://image.beloved.ink/Typora/image-20200508133311445.png)

关闭KDUMP

![image-20200508133440724](http://image.beloved.ink/Typora/image-20200508133440724.png)

![image-20200508133507319](http://image.beloved.ink/Typora/image-20200508133507319.png)

设置时间

![image-20200508133610900](http://image.beloved.ink/Typora/image-20200508133610900.png)

![image-20200508133726851](http://image.beloved.ink/Typora/image-20200508133726851.png)

**一定要打开网络，要不然后期特别麻烦**

![image-20200508133822936](http://image.beloved.ink/Typora/image-20200508133822936.png)

![image-20200508133851470](http://image.beloved.ink/Typora/image-20200508133851470.png)

按需选择需要安装的软件

![image-20200508134251613](http://image.beloved.ink/Typora/image-20200508134251613.png)

![image-20200508134310842](http://image.beloved.ink/Typora/image-20200508134310842.png)

开始安装

![image-20200508134341126](http://image.beloved.ink/Typora/image-20200508134341126.png)

在等待安装时可以配置一下root的密码和用户

![image-20200508134413823](http://image.beloved.ink/Typora/image-20200508134413823.png)

这里我先只设置root的密码，用户等会创建

![image-20200508134453301](http://image.beloved.ink/Typora/image-20200508134453301.png)

安装完成。重启

![image-20200508135733883](http://image.beloved.ink/Typora/image-20200508135733883.png)

接受许可证

![image-20200508135931415](http://image.beloved.ink/Typora/image-20200508135931415.png)

![image-20200508135957357](http://image.beloved.ink/Typora/image-20200508135957357.png)

创建用户

![image-20200508140030233](http://image.beloved.ink/Typora/image-20200508140030233.png)

![image-20200508140140183](http://image.beloved.ink/Typora/image-20200508140140183.png)

结束配置

登录刚才创建的用户

![image-20200508140242474](http://image.beloved.ink/Typora/image-20200508140242474.png)

语言选择

![image-20200508140322298](http://image.beloved.ink/Typora/image-20200508140322298.png)

![image-20200508140349108](http://image.beloved.ink/Typora/image-20200508140349108.png)

根据自己的需求选择

![image-20200508140438574](http://image.beloved.ink/Typora/image-20200508140438574.png)

至此安装完成

# 配置

查看有没有网络

打开终端ping一下百度

![image-20200508140724996](http://image.beloved.ink/Typora/image-20200508140724996.png)

有网，可以ping通

**修改IP为静态IP**

查看ip地址`ifconfig`

![image-20200508141528571](http://image.beloved.ink/Typora/image-20200508141528571.png)

现在的ip地址是随机分配的，下次开机地址会变，为了以后方便使用，讲ip地址修改为静态的

修改网卡配置

切换为root用户`su root`

![image-20200508142848955](http://image.beloved.ink/Typora/image-20200508142848955.png)

网卡配置文件在`/etc/sysconfig/network-scripts/`目录

使用vim修改配置文件

```bash
vim /etc/sysconfig/network-scripts/ifcfg-ens160 
```

![image-20200508143542558](http://image.beloved.ink/Typora/image-20200508143542558.png)

修改如下：

```bash
1.  TYPE="Ethernet"  # 网络类型为以太网
2.  BOOTPROTO="static" # 手动分配ip的意思，dhcp是自动获取
3.  NAME="ens160" # 网卡设备名，设备名一定要跟文件名一致
4.  DEVICE="ens160" # 网卡设备名，设备名一定要跟文件名一致
5.  ONBOOT="yes" # 该网卡是否随网络服务启动
6.  IPADDR="192.168.3.123" # 该网卡ip地址就是你要配置的固定IP，如果你要用xshell等工具连接，1这个网段需要你自己的电脑网段一致，否则有可能用xshell连接失败
7.  GATEWAY="192.168.3.1"  # 网关
8.  NETMASK="255.255.255.0"  # 子网掩码，可以不配置
9.  DNS1="119.29.29.29"  # DNS 119.29.29.29 为腾讯的dns
```

重启网卡

```bash
nmcli c reload
```

ping网络测试，没有问题

![image-20200508144222568](http://image.beloved.ink/Typora/image-20200508144222568.png)

# **使用Xshell连接**

![image-20200508144408974](http://image.beloved.ink/Typora/image-20200508144408974.png)

输入用户名，密码

![image-20200508144612511](http://image.beloved.ink/Typora/image-20200508144612511.png)

![image-20200508144458240](http://image.beloved.ink/Typora/image-20200508144458240.png)

连接成功

![image-20200508144629302](http://image.beloved.ink/Typora/image-20200508144629302.png)

测试

![image-20200508144709393](http://image.beloved.ink/Typora/image-20200508144709393.png)