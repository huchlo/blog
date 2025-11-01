
# redis-server

```bash
./redis-server #默认配置
./redis-server redis.conf
```
- 后台启动redis服务
编辑conf文件，将daemonize属性改为yes（表明需要在后台运行）
再次启动redis服务ok

# redis-cli
```bash
redis-cli
redis-cli -h {host,默认127.0.0.1} -p {port,默认6379}
redis-cli {command,不输入则进入redis控制台}
redis-cli -r {n,重复n次} -i {s,隔s秒执行}
redis-cli -c # 连接集群时使用
redis-cli -a {password}

echo "world" | redis-cli -x set hello #从标准输入读取数据作为该命令的最后一个参数
redis-cli set hello world

```

# utils/redis_init_script
linux系统启动时，redis初始化脚本的模板，编辑后cp到/etc/init.d下，主要修改REDISPORT、EXEC、CLIEXEC、CONF这几个参数，之后一般会把该脚本名mv成redis_{port}
```bash
#使用chkconfig命令，将redis添加进开机启动
chkconfig --list # 查看服务

#0：表示关机
#1：单用户模式
#2：无网络连接的多用户命令行模式
#3：有网络连接的多用户命令行模式
#4：不可用
#5：带图形界面的多用户模式
#6：重新启动

chkconfig --add redis_6379 #添加进服务
chkconfig redis_6379 on #设置开机启动，no对运行级2345有效
# chkconfig [--level <等级代号>][系统服务][on/off/reset] 

chkconfig --del name #删除服务
```


# console
 Redis常用命令手册 http://c.biancheng.net/redis_command/
```bash

redis> set foo bar
OK
redis> get foo
"bar"

info [section] #返回redis服务器的各种信息,section 可选项
#Server 服务器相关信息
#Clients 客户端相关信息
#Memory 服务器内存
#Persistence 持久化信息
#Stats 通用统计数据
#Replication 主从复制相关信息
#CPU cpu使用情况
#Cluster 集群信息
#KeySpace 键值对统计数量信息

slaveof 10.0.0.3 6379 # 成为10.0.0.3的从机
slaveof no one # slave转master


cluster info # 查看集群状态
cluster nodes # 列出集群当前已知的所有节点


SCAN 0 MATCH abc:* #查看以asd:开头的key个数

```

# 安装
[Redis 安装_redis教程](https://www.redis.net.cn/tutorial/3503.html)
```bash
wget http://download.redis.io/releases/redis-5.0.4.tar.gz
tar xzf redis-5.0.4.tar.gz
cd redis-5.0.4
make
#执行`make install`命令后，默认情况下，Redis将被安装到`/usr/local/bin`目录下 
```
编译依赖gcc，检查：`rpm -qa | grep gcc`，安装：`yum -y gcc`
编译完成后，在Src目录下，有四个可执行文件redis-server、redis-benchmark、redis-cli和redis.conf。然后拷贝到一个目录下。之前的包就没用了，只要这些文件，且若没设置则数据存在当前目录
```bash
mkdir /usr/redis
cp redis-server  /usr/redis
cp redis-benchmark /usr/redis
cp redis-cli  /usr/redis
cp redis.conf  /usr/redis
cd /usr/redis
```

# 解决redis远程连接不上的问题
redis现在的版本开启redis-server后，redis-cli只能访问到127.0.0.1，因为在配置文件中固定了ip，因此需要修改redis.conf文件以下几个地方。

1. bind 127.0.0.1改为 # bind 127.0.0.1 或 bind 0.0.0.0
2. protected-mode yes 改为 protected-mode no

验证方式
```bash
[root@mch ~]# ps -ef | grep redis
　　root      2175     1  0 08:15 ?        00:00:05 /usr/local/bin/redis-server *:6379
```
通过"\*"就可以看出此时是允许所有的ip连接登录到这台redis服务上

# redis.conf
http://t.zoukankan.com/dw3306-p-12801566.html
```bash
#安全通用设置
bind 10.0.0.1 127.0.0.1 #访问通过这些ip，才被允许，0.0.0.0 代表所有
requirepass password #密码
protected-mode yes #守护进程模式，默认开启
port 6379 #端口，默认6379

daemonize yes #后台运行
databases 16 #数据库数量
pidfile /var/run/redis_6379.pid #redis的进程文件

#集群相关
cluster-enabled yes #集群开关
cluster-config-file nodes-6379.conf #集群配置文件，不用手动配置，会自动生成
cluster-node-timeout 15000 #互联超时阀值，毫秒

# cluster-node-timeout 15000
# cluster-replica-validity-factor 10
```

# 集群
普通主从，多slave主动同步单master数据，slave只读，master挂了就不能写入数据了，没有容错性，不予采用。
## Sentinel 模式
在普通主从的基础上，启用单数个sentinel对redis集群的运行状况进行监控，当master挂点时，sentinel集群会选举一个slave成为master。


## Cluster 模式

```bash
# /redis-cli --cluster 

#创建集群
redis-cli --cluster create --cluster-replicas 1 10.12.58.7:6379 10.12.58.7:6380 10.12.58.9:6379 10.12.58.9:6380 10.12.58.10:6379 10.12.58.10:6380

#检查集群
redis-cli --cluster check 192.168.163.132:6384 --cluster-search-multiple-owners

#查看集群
redis-cli --cluster info 10.0.0.3:6379

```

```bash
[app@localhost redis]<20220713 11:15:17>$ bin/redis-cli --cluster create --cluster-replicas 1 10.12.58.7:6379 10.12.58.7:6380 10.12.58.9:6379 10.12.58.9:6380 10.12.58.10:6379 10.12.58.10:6380 -a 'Z%(OSinLCi!sk*X3h#REDIS'
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 10.12.58.9:6380 to 10.12.58.7:6379
Adding replica 10.12.58.7:6380 to 10.12.58.9:6379
Adding replica 10.12.58.10:6380 to 10.12.58.10:6379
>>> Trying to optimize slaves allocation for anti-affinity
[OK] Perfect anti-affinity obtained!
M: c22039dbf7f987668a537f92cec4863ed91122b8 10.12.58.7:6379
   slots:[0-5460] (5461 slots) master
S: 752bb07d902bf7fb8d760a7aaf1755c34ce062d9 10.12.58.7:6380
   replicates c837f0d2ae44271a0dec546db0c72fbe0b4d0cfa
M: c837f0d2ae44271a0dec546db0c72fbe0b4d0cfa 10.12.58.9:6379
   slots:[5461-10922] (5462 slots) master
S: 3da00450f06b132ac7c3d9ae6069721a3ee51dea 10.12.58.9:6380
   replicates 02f0239c1014a51a590d4c0736aeb20a2b10b834
M: 02f0239c1014a51a590d4c0736aeb20a2b10b834 10.12.58.10:6379
   slots:[10923-16383] (5461 slots) master
S: 8db1e4b294f83be39ca54a51447b25d1c6eac833 10.12.58.10:6380
   replicates c22039dbf7f987668a537f92cec4863ed91122b8
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join
.....
>>> Performing Cluster Check (using node 10.12.58.7:6379)
M: c22039dbf7f987668a537f92cec4863ed91122b8 10.12.58.7:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
S: 752bb07d902bf7fb8d760a7aaf1755c34ce062d9 10.12.58.7:6380
   slots: (0 slots) slave
   replicates c837f0d2ae44271a0dec546db0c72fbe0b4d0cfa
M: 02f0239c1014a51a590d4c0736aeb20a2b10b834 10.12.58.10:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
S: 8db1e4b294f83be39ca54a51447b25d1c6eac833 10.12.58.10:6380
   slots: (0 slots) slave
   replicates c22039dbf7f987668a537f92cec4863ed91122b8
S: 3da00450f06b132ac7c3d9ae6069721a3ee51dea 10.12.58.9:6380
   slots: (0 slots) slave
   replicates 02f0239c1014a51a590d4c0736aeb20a2b10b834
M: c837f0d2ae44271a0dec546db0c72fbe0b4d0cfa 10.12.58.9:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

```


```bash
daemonize yes

#绑定IP和端口看这里
bind 0.0.0.0
port 6379

#db文件修改看这里
dbfilename "dump_6379.rdb"
dir "/tmp"
rdbcompression yes
rdbchecksum yes

databases 16
#rdb
save 900 1
save 300 10
save 60 10000

#TCP 监听的最大容纳数量
tcp-backlog 511
timeout 0
tcp-keepalive 60

#开启appendonly看这里
appendonly no
appendfilename "appendonly.aof"
#aof
appendfsync everysec
no-appendfsync-on-rewrite no
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb

#最大内存改这里
#maxmemory 8GB
#maxmemory-policy volatile-lru

#绑定IP和端口看这里

#pidfile修改看这里
pidfile "/tmp/redis_6379.pid"
#log修改看这里
loglevel notice
logfile "/tmp/redis_6379.log"

#rdb文件修改看这里

save 900 1
save 300 10
save 60 10000

#TCP 监听的最大容纳数量

#开启appendonly看这里

#最大内存改这里
#maxmemory 8GB
#maxmemory-policy volatile-lru

#客户端最大连接数改这里
#maxclients 10000

#主备配置改这里
#slaveof ip:port
#slave-serve-stale-data yes
#slave-priority 100

#密码认证改这里
#masterauth redis
#requirepass redis

#集群配置改这里
cluster-enabled yes
cluster-node-timeout 15000
cluster-config-file "nodes.conf"

#其他优化配置
lua-time-limit 5000
slowlog-log-slower-than 10000
slowlog-max-len 128
latency-monitor-threshold 0
hash-max-ziplist-entries 512
hash-max-ziplist-value 64
list-max-ziplist-size -2
list-compress-depth 0
set-max-intset-entries 512
zset-max-ziplist-entries 128
zset-max-ziplist-value 64
hll-sparse-max-bytes 3000
activerehashing yes
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit replica 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60
hz 10
aof-rewrite-incremental-fsync yes
# Generated by CONFIG REWRITE
masterauth "Z%(OSinLCi!sk*X3h#REDIS"
requirepass "Z%(OSinLCi!sk*X3h#REDIS"

```

<<<<<<< HEAD
# 持久化
redis的持久化分AOP和RDB两种，AOP是在增量的将数据操作同步到文件（比较实时），RDB是将内存的快照保存到文件，可同时使用

```bash
#aop触发方式
#在redis.conf中
appendonly yes
appendfilename "appendonly.aof"
appenddirname "appendonlydir"
appendfsync everysec
no-appendfsync-on-rewrite no
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
aof-load-truncated yes
aof-use-rdb-preamble yes
aof-timestamp-enabled no

#rdb触发方式
#在redis-cli中
SAVE #直接保存，会阻塞主线程
BGSAVE #生一个子进程保存，不会阻塞主线程
#在redis.conf中
save 900 1 #900秒内至少发生1次写操作
save 300 10 #300秒内至少发生10次写操作
save 60 10000 #60秒内至少发生10000次写操作时

save 3600 1 300 100 60 10000
stop-writes-on-bgsave-error yes
rdbcompression yes
rdbchecksum yes
dbfilename dump.rdb
rdb-del-sync-files no
dir /var/lib/redis

```

正常恢复： 
修改redis.windows.conf中的appendonly no，改为yes 
将有数据的aof⽂件复制⼀份到对应⽬录（利⽤config get dir查看⽬录） 
重启Redis重新加载即可⾃动恢复数据

RDB的备份与恢复 
先通过config get dir指令查询rdb⽂件的⽬录 
关闭redis服务 
将备份的快照⽂件（如dump.rdb）拷⻉到查询出来的⽬录下 
启动Redis，备份的快照数据将会被直接加载
=======
# 数据持久化

RDB 持久化（Redis Database Backup file）、 AOF 持久化（Append Only File）

|   | RDB   | AOF   |
| --- | --- | --- |
|  持久化方式  | 定时对整个内存做快照   | 记录每一次执行的命令   |
| 数据完整性   | 不完整，两次备份之间会丢失   | 相对完整，取决于刷盘策略   |
| 文件大小   | 会有压缩，文件体积大小   | 记录命令，文件体积大   |
| 宕机回复速度   | 很快   | 慢   |
| 数据恢复优先级   | 低   | 高   |
| 系统资源占用   | 高，大量的cpu和内存消耗   | 低，主要是磁盘io，但aof重写时会占用大量cpu和内存资源   |
| 使用场景   | 可以容忍数分钟的数据丢失，更快的启动速度   | 对数据完整性要求较高   |


```bash
dbfilename "dump_6379.rdb"
dir "/tmp"
rdbcompression yes
rdbchecksum yes
#rdb
save 900 1 #在900秒(15分钟)之后，如果至少有1个key发生变化，则dump内存快照
save 300 10 #在300秒(5分钟)之后，如果至少有10个key发生变化，则dump内存快照
save 60 10000 #在60秒(1分钟)之后，如果至少有10000个key发生变化，则dump内存快照
```

```bash
#开启appendonly看这里
appendonly yes
appendfilename "appendonly.aof"
#aof
appendfsync everysec #每秒钟同步一次，该策略为AOF的缺省策略
#appendfsync no #从不同步
#appendfsync always #每次有数据修改发生时都会写入AOF文件
no-appendfsync-on-rewrite no
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
```
>>>>>>> 86739afc8b075d45aea05c107fe45253c67db021
