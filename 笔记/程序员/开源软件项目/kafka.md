[apache kafka系列之server.properties配置文件参数说明_幽灵之使的博客-CSDN博客_server.properties 配置](https://blog.csdn.net/lizhitao/article/details/25667831)


# bin/kafka-server-\*.sh
```bash
#启动服务
bin/kafka-server-start.sh -daemon config/server.properties
bin/kafka-server-start.sh -daemon config/kraft/server.properties
#停止服务
bin/kafka-server-stop.sh
```

# bin/kafka-topics.sh
>连接主机 --bootstrap-server <string:server to connect to>
>操作topic的名称 --topic <String:name>
>增 --create
>删 --delete
>改--alert
>查 --list
>查一个--describe
>分区数 --partitions <Integer: # of partitons>
>分区副本数 --replication-factor \<integer\>
>更新系统默认配置 --config \<string\>

分区只能增加不能减少，副本不能通过指令的方式修改

```bash
bin/kafka-topics.sh --bootstrap-server com01:9092 --topic first --create --partitions 1 --replication-factor 1
bin/kafka-topics.sh --bootstrap-server com01:9092 --list
bin/kafka-topics.sh --bootstrap-server com01:9092 --topic first --describe
bin/kafka-topics.sh --bootstrap-server com01:9092 --topic first --alter --partitions 2
```

# config/server.properties

## kafka-kraft模式
修改config/kraft下的配置文件
>node.id
>controller.quorum.vters=node.id@ip:9093
>advertised.listeners
>log.dirs

初始化数据目录
生成存储目录唯一id

```bash
bin/kafka-storage.sh random-uuid
```

用该id格式化kafka存储目录

```bash
bin/kafka-storage.sh format -t 6-_j0MqeQZ-zgyui5Mlw4w -c /opt/kafka_2.12-3.1.0/config/kraft/server.properties
```


# 认识
作用：缓存/消峰，解耦，异步通讯
## broker

一个服务节点

## topic
即一个主题，是对消息的分类，一个主题对应N个分区（partition），N个副本（replica）
- partition
将一个topic的数据分成数个partition，partition又可以分散到各个broker，produer按照一定的算法把消息发送给各个partition上，consumer按照一定的算法去消费不同pratition上的消息，每一个pratition最大允许一个consumer去消费，一个consumer可以消费多个pratition。

pratition越多，但broker的消息吞吐量越大，但过多的pratition会降低集群的不可用性，如当一个broker计划停止服务时，kafka会将broker上的所有leader一个个移走，每次移动都会导致服务几毫秒时间不可用，当非计划停止服务时，broker上的pratition会全部不可用。

建议将每个broker的partition限制在2000-4000，每个kafka集群中的partition限制在10000。
- replica
每个partition的副本数量，存在的唯一目的就是防止数据丢失，副本分为两类：领导者副本（leader replica）和追随者副本（follower replica）。follower replica 是不能提供服务给客户端的，也就是说不负责响应客户端发来的消息写入和消息消费请求。它只是被动地向领导者副本（leader replica）获取数据，而一旦 leader replica 所在的 broker 宕机，Kafka 会从剩余的 replica 中选举出新的 leader 继续提供服务。


## console-producer

```bash
bin/kafka-console-producer.sh --bootstrap-server com01:9092 --topic first
echo 123 | bin/kafka-console-producer.sh --broker-list 10.12.58.3:9092 --topic test 
echo asd 123 | bin/kafka-console-producer.sh --broker-list 10.12.58.3:9092 --topic test --property parse.key=true
```


## console-consumer

```bash
bin/kafka-console-consumer.sh --bootstrap-server com01:9092 -topic first --from-beginning
```

#待整理