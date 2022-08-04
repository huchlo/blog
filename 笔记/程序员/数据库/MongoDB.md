安装：https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-red-hat/

# 启动服务
进入bin中执行
```bash
mongod --dbpath "db文件夹的url"
```
在浏览器用http://localhost:27017/查看数据是否启动

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



mongodb://localhost:27017/?readPreference=primary&appname=MongoDB%20Compass&directConnection=true&ssl=false
#待整理