[Install MongoDB Community Edition on Red Hat or CentOS — MongoDB Manual](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-red-hat/)

# 设置dbpath
进入bin中执行
```bash
mongod --dbpath "db文件夹的url"
```
在浏览器用http://localhost:27017/查看数据是否启动

# 认识
MongoDB按文档（json）的形式存储数据，相较于sql数据库存储“扁平”的表数据，具有存储“立体”数据的优势

#待整理