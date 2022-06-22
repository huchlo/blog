[apache kafka系列之server.properties配置文件参数说明_幽灵之使的博客-CSDN博客_server.properties 配置](https://blog.csdn.net/lizhitao/article/details/25667831)

# kafka-kraft模式
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

启动
```bash
bin/kafka-server-start.sh -daemon config/kraft/server.properties
```

# 认识
作用：缓存/消峰，解耦，异步通讯

启动
```bash
bin/kafka-server-start.sh -deamon config/kraft/server.properties
```
停止
```bash
bin/kafka-server-stop.sh
```

## topic

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

bin/kafka-topics.sh --bootstrap-server com01:9092 --topic first --create --partitions 1 --replication-factor 1
bin/kafka-topics.sh --bootstrap-server com01:9092 --list
bin/kafka-topics.sh --bootstrap-server com01:9092 --topic first --describe
bin/kafka-topics.sh --bootstrap-server com01:9092 --topic first --alter --partitions 2

## console-producer

```bash
bin/kafka-console-producer.sh --bootstrap-server com01:9092 -topic first
```


## console-consumer

```bash
bin/kafka-console-producer.sh --bootstrap-server com01:9092 -topic first
bin/kafka-console-consumer.sh --bootstrap-server com01:9092 -topic first --from-beginning
```

