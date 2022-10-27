
[索引 - Apache ZooKeeper - Apache Software Foundation](https://cwiki.apache.org/confluence/display/ZOOKEEPER)

# bin/zkServer.sh

```bash
#默认使用conf/zoo.cfg
bin/zkServer.sh {start|start-foreground|stop|restart|status|upgrade|print-cmd}
#指定配置文件
bin/zkServer.sh start conf/my_zoo.cfg
```

# conf/zoo.cfg

```bash
# The number of milliseconds of each tick
tickTime=2000
# The number of ticks that the initial
# synchronization phase can take
initLimit=10
# The number of ticks that can pass between
# sending a request and getting an acknowledgement
syncLimit=5
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just
# example sakes.
dataDir=/tmp/zookeeper
# the port at which the clients will connect
clientPort=2181
# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60
#
# Be sure to read the maintenance section of the
# administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3
# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1

#集群
#x为整数，是ZooKeeper中服务器的一个简单标识，这个数值需要和dataDir下的myid文件内容一致
#hostname：ZooKeeper服务器节点的地址
#port_A：端口，用于Follower和Leader之间的数据同步和其它通信
#port_B：端口，用于Leader选举过程中投票通信
#serverl.x = [hostname]:port_A:port_B
server.1=10.0.0.3:2888:3888
server.2=10.0.0.4:2888:3888
server.3=10.0.0.5:2888:3888
server.4=10.0.0.6:2888:3888

#observer节点
peerType=observer
server.1=localhost:2181:3181:observer

#group.x=A[:B]
#x为整数值，分组id
#A、B为服务器节点
#不配置的归入默认组
group.1=1
group.2=2:3:4

#weight.x=N
#x为整数值，服务器节点
#N每个服务器节点指定权重值，权重越高，投票时能投的票数越多,默认为1
weight.1=2
weight.2=1
weight.3=1
weight.4=1
```
[【ZooKeeper】配置文件详解_Young丶的博客-CSDN博客_zookeeper配置文件详解](https://blog.csdn.net/agonie201218/article/details/114637475)

# bin/zkCli.sh
```bash
#-server 默认 127.0.0.1:2181
bin/zkCli.sh -server hostname:2181
```
```bash
ls /zookeeper
get /zookeeper/config
```

# 集群注意项
- 集群特性
集群中要有**过半**的机器正常工作，那么整个集群才是可用的。
- 节点数为最好为奇数
因为3集群只能挂1台，4集群也只能挂1台。

推荐集群方案：1 leader + 2 follower + 2 observer
[集群没有leader_运维必备：Zookeeper集群“脑裂”问题处理大全_weixin_39745013的博客-CSDN博客](https://blog.csdn.net/weixin_39745013/article/details/111697774)

# 认识
节点信息储存和管理