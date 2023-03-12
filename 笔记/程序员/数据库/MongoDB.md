# 安装
https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-red-hat/

# 启动服务
进入bin中执行
```bash
mongod --dbpath "db文件夹的url"
/usr/bin/mongod -f /etc/mongod.conf #指定配置文件启动
#高版本不适用-f
./mongodb-linux-x86_64-rhel80-6.0.2/bin/mongod --config mongod.conf
```
在浏览器用http://localhost:27017/查看数据是否启动

```yml
#mongod.conf
systemLog:
  destination: file
  logAppend: true
  path: /home/app/huangchenglong/mongod.log

storage:
  dbPath: /home/app/huangchenglong/mongod_dbpath
  journal:
    enabled: true

processManagement:
  fork: true  # fork and run in background
  pidFilePath: /home/app/huangchenglong/mongod.pid  # location of pidfile

# network interfaces
net:
  port: 27017
  bindIp: 0.0.0.0  # Listen to local interface only, comment to listen on all interfaces.

```
# 认识
MongoDB按文档（json）的形式存储数据，相较于sql数据库存储“扁平”的表数据，具有存储“立体”数据的优势

# 连接参数

```console
mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]
```

-   **mongodb://** 这是固定的格式，必须要指定。
-   **username:password@** 可选项，如果设置，在连接数据库服务器之后，驱动都会尝试登录这个数据库
-   **host1** 必须的指定至少一个host, host1 是这个URI唯一要填写的。它指定了要连接服务器的地址。如果要连接复制集，请指定多个主机地址。
-   **portX** 可选的指定端口，如果不填，默认为27017
-   **/database** 如果指定username:password@，连接并验证登录指定数据库。若不指定，默认打开 test 数据库。
-   **?options** 是连接选项。如果不使用/database，则前面需要加上/。所有连接选项都是键值对name=value，键值对之间通过&或;（分号）隔开

>readPreference=**primary** 只从primary节点读取数据(默认)
>readPreference=**primaryPreferred** 优先从primary读取，primary不可用时从secondary读
>readPreference=**secondary** 只从副本集中secondary节点读数据
>readPreference=****secondaryPreferred**** 优先从secondary读取，如果secondary不可用时就从primary读
>readPreference=****nearest**** 根据网络距离，就近读取，根据客户端与服务端的PingTime是实现

directConnection=true 直连
ssl=false 不启用ssl

示例：mongodb://localhost:27017/?readPreference=primary&appname=MongoDB%20Compass&directConnection=true&ssl=false

# 命令行操作mongodb

1. 连接
`mongosh`
`/usr/bin/mongo*`
2. 基本命令
```bash
db.version() #版本

show dbs #查看所有数据库
use xxx #使用xxx数据库，可以使用不存在的数据库，当insert的时候会自动创建
db #查看当前使用的数据库名
db.dropDatabase() #删除当前数据库
show collections/tables #查看当前数据库的所有集合
db.createCollection("asd") #当前数据库下创建集合
db.getCollectionInfos() #查看数据库的集合信息

db.asd.drop() #删除asd集合
db.asd.insert({"asd":123}) #当前数据库下的asd(没有则创建)集合中，插入一条数据
db.asd.find() #按条件查找数据
db.asd.find().pretty() #查找后漂亮的输出
db.asd.update({'_id':ObjectId("6334fbe2ff6c3534c24730a2")},{$set:{'asd':123}}) #修改一条数据
db.asd.update(<query>,<update>,{upsert: <boolean>,multi: <boolean>,writeConcern: <document>}) #upsert,不存在符合的query是否插入,默认为alse;是否批量,默认false,只更新找到的第一条记录
db.asd.remove({}) #删除所有数据
db.asd.remove({'asd':123}) #删除符合的所有数据
db.asd.remove(<query>,justOne<boolean>) #删除数据,justOne,是否只删除一个文档，默认为false
```

3. 备份与恢复
```bash
#备份 数据库
mongodump -h 127.0.0.1:27017 -d 数据库名 -o /home/data
#恢复 数据库
mongorestore -h 127.0.0.1:27017 -d 恢复的数据库名 /home/data

#导出 单张数据表，-f是指定字段导出
mongoexport -h 127.0.0.1:27017 -d 数据库名 -c 表名 -o /home/data/user.json
mongoexport -h 127.0.0.1:27017 -d 数据库名 -c 表名 -o /home/data/user.json -f "_id, username, password"
#导入 单张数据表
mongoimport -h 127.0.0.1:27017 -d 数据库名 -c 表名 /home/data/user.json
```

[mongodump — MongoDB Database Tools](https://www.mongodb.com/docs/database-tools/mongodump/#mongodb-binary-bin.mongodump)
