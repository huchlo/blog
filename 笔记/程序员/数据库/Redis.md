
# 启动 redis-server

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
#Replication 蛀虫复制相关信息
#CPU cpu使用情况
#Cluster 集群信息
#KeySpace 键值对统计数量信息
```

# 安装
[Redis 安装_redis教程](https://www.redis.net.cn/tutorial/3503.html)
```bash
wget http://download.redis.io/releases/redis-5.0.4.tar.gz
tar xzf redis-5.0.4.tar.gz
cd redis-5.0.4
make
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
protected-mode yes #默认开启
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
普通主从，多slave主动同步单master数据，slave只读，master挂了就不能写入数据了，没有容错性，不予学习。
## Sentinel 模式

## Cluster 模式
