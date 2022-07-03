
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
```console

#bind 127.0.0.1
protected-mode no
port 6379
tcp-backlog 511
timeout 0
tcp-keepalive 300
daemonize yes
supervised no
pidfile /var/run/redis_6379.pid
loglevel notice

save 900 1
save 300 10
save 60 10000

# cluster-enabled yes
# cluster-config-file nodes-6379.conf
# cluster-node-timeout 15000
# cluster-replica-validity-factor 10
```

# 集群注意项
#待整理