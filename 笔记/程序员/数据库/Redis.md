# 安装
[Redis 安装_redis教程](https://www.redis.net.cn/tutorial/3503.html)
```bash
wget
tar
cd
make
```
编译完成后，在Src目录下，有四个可执行文件redis-server、redis-benchmark、redis-cli和redis.conf。然后拷贝到一个目录下。之前的包就没用了，只要这些文件，且若没设置则数据存在当前目录
```bash
mkdir /usr/redis
cp redis-server  /usr/redis
cp redis-benchmark /usr/redis
cp redis-cli  /usr/redis
cp redis.conf  /usr/redis
cd /usr/redis
```

# 启动 redis-server

```bash
./redis-server #默认配置
./redis-server redis.conf
```
后台启动redis服务
编辑conf文件，将daemonize属性改为yes（表明需要在后台运行）
再次启动redis服务ok
# redis-cli
```bash
redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```

# 解决redis远程连接不上的问题
redis现在的版本开启redis-server后，redis-cli只能访问到127.0.0.1，因为在配置文件中固定了ip，因此需要修改redis.conf（有的版本不是这个文件名，只要找到相对应的conf后缀的文件即可）文件以下几个地方。
1.bind 127.0.0.1改为 # bind 127.0.0.1 或 bind 0.0.0.0
2.protected-mode yes 改为 protected-mode no
验证方式
```bash
[root@mch ~]# ps -ef | grep redis
　　root      2175     1  0 08:15 ?        00:00:05 /usr/local/bin/redis-server *:6379
```
通过"\*"就可以看出此时是允许所有的ip连接登录到这台redis服务上
