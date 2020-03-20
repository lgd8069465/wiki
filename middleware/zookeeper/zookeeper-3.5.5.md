[Licensed under the CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)

在此感谢 [@zookeeper](https://zookeeper.apache.org/) [@oracle](https://www.oracle.com/index.html)

文档不定时更新，如有疑问请及时联系本文作者，当前文档版本1.0.0

[apache-zookeeper-3.5.5-bin.tar.gz](https://downloads.apache.org/zookeeper/zookeeper-3.5.5/apache-zookeeper-3.5.5-bin.tar.gz)

[jdk-8u221-linux-x64.tar.gz](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
~~~
// 操作系统版本
[root@docker opt]# cat /etc/centos-release
CentOS Linux release 7.7.1908 (Core)
// 关闭selinux
[root@docker opt]# vi /etc/selinux/config
SELINUX=disabled
// 重启
[root@docker opt]# reboot
// 设置主机名
[root@docker opt]# vi /etc/hostname
docker
// 查看IP
[root@docker opt]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:1c:6e:f5 brd ff:ff:ff:ff:ff:ff
    inet 192.168.192.129/24 brd 192.168.192.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::e7f8:8f77:9df8:ed25/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 02:42:85:25:9c:dc brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
// 设置hosts
[root@docker opt]# vi /etc/hosts
192.168.192.129 docker
// 关闭防火墙
[root@docker opt]# systemctl stop firewalld
// 关闭自启动防火墙
[root@docker opt]# systemctl disable firewalld
// 解压软件
[root@docker opt]# tar zxf apache-zookeeper-3.5.5-bin.tar.gz
[root@docker opt]# tar zxf jdk-8u221-linux-x64.tar.gz
// 创建java目录
[root@docker opt]# mkdir -pv /usr/java/
mkdir: created directory ‘/usr/java/’
// 移动java
[root@docker opt]# mv jdk1.8.0_221 /usr/java/
// 设置环境变量
[root@docker opt]# vi /etc/profile
export JAVA_HOME=/usr/java/jdk1.8.0_221
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=.:$JRE_HOME/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
// 及时生效
[root@docker opt]# source /etc/profile
// 验证java
[root@docker opt]# java
[root@docker opt]# javac
[root@docker opt]# java -version
java version "1.8.0_221"
Java(TM) SE Runtime Environment (build 1.8.0_221-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.221-b11, mixed mode)
[root@docker opt]# echo $JAVA_HOME
/usr/java/jdk1.8.0_221
[root@docker opt]# echo $JRE_HOME
/usr/java/jdk1.8.0_221/jre
// 拷贝配置文件
[root@docker opt]# cp apache-zookeeper-3.5.5-bin/conf/zoo_sample.cfg apache-zookeeper-3.5.5-bin/conf/zoo.cfg
// 编辑配置文件
[root@docker opt]# vi apache-zookeeper-3.5.5-bin/conf/zoo.cfg
dataDir=/home/zookeeper
clientPort=2181
// 创建数据目录
[root@docker opt]# mkdir -p /home/zookeeper
[root@docker opt]# cd apache-zookeeper-3.5.5-bin/bin
[root@docker bin]# zkServer.sh start
[root@docker bin]# zkServer.sh stop
[root@docker bin]# zkServer.sh restart
[root@docker opt]# zkServer.sh status
~~~
## zookeeper-cluster
~~~
[root@docker opt]# vi apache-zookeeper-3.5.5-bin/conf/zoo.cfg
dataDir=/home/zookeeper
clientPort=2181
server.0=192.168.192.128:2888:3888
server.1=192.168.192.129:2888:3888
server.2=192.168.192.134:2888:3888
// 192.168.192.128
[root@docker opt]# vi dataDir=/home/zookeeper/myid
0
// 192.168.192.129
[root@docker opt]# vi dataDir=/home/zookeeper/myid
1
// 192.168.192.134
[root@docker opt]# vi dataDir=/home/zookeeper/myid
2

// 192.168.192.128
[root@docker conf]# /opt/apache-zookeeper-3.5.5-bin/bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /opt/apache-zookeeper-3.5.5-bin/bin/../conf/zoo.cfg
Client port found: 2181. Client address: 192.168.192.128.
Mode: follower
// 192.168.192.129
[root@docker conf]# /opt/apache-zookeeper-3.5.5-bin/bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /opt/apache-zookeeper-3.5.5-bin/bin/../conf/zoo.cfg
Client port found: 2181. Client address: 192.168.192.129.
Mode: leader
// 192.168.192.134
[root@docker conf]# /opt/apache-zookeeper-3.5.5-bin/bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /opt/apache-zookeeper-3.5.5-bin/bin/../conf/zoo.cfg
Client port found: 2181. Client address: 192.168.192.134.
Mode: follower
~~~
## zookeeper-cluster(伪分布式)
~~~
[root@docker opt]# vi apache-zookeeper-3.5.5-bin1/conf/zoo.cfg
dataDir=/home/zookeeper1
clientPort=2182
server.0=192.168.192.128:2888:3888
server.1=192.168.192.129:2887:3887
server.2=192.168.192.134:2886:3886
// 192.168.192.128
[root@docker opt]# vi dataDir=/home/zookeeper/myid
0
// 192.168.192.129
[root@docker opt]# vi dataDir=/home/zookeeper1/myid
1
// 192.168.192.134
[root@docker opt]# vi dataDir=/home/zookeeper2/myid
2

[root@docker opt]# /opt/apache-zookeeper-3.5.5-bin/bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /opt/apache-zookeeper-3.5.5-bin/bin/../conf/zoo.cfg
Client port found: 2181. Client address: localhost.
Mode: follower
[root@docker opt]# /opt/apache-zookeeper-3.5.5-bin1/bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /opt/apache-zookeeper-3.5.5-bin1/bin/../conf/zoo.cfg
Client port found: 2182. Client address: localhost.
Mode: leader
[root@docker opt]# /opt/apache-zookeeper-3.5.5-bin2/bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /opt/apache-zookeeper-3.5.5-bin2/bin/../conf/zoo.cfg
Client port found: 2183. Client address: localhost.
Mode: follower
[root@docker opt]#
~~~