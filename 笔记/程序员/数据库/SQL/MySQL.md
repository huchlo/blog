官网： https://www.mysql.com/  
开发者文档： https://dev.mysql.com/doc/

# 优化
## SQL优化
- 深分页
```sql
SELECT * FROM `talbe_name` LIMIT 100000,10
```
offset大于10w，性能有明显的下降，建议使用id
```sql
SELECT * FROM `talbe_name` WHERE id > 100000 LIMIT 10
```

## 其他
对于使用逻辑删除的相关表查询，最好使用视图

# JSON数据类型
```sql
select * from table_name where json_value -> '$.xxx[0].XXX' = 'string'
```
`json_value -> '$.xxx'` 相当于字段
```sql
select JSON_INSERT("{\"asda\":23}","$.asd","新增数据")
```
```sql
select JSON_ARRAY_APPEND("{\"contact\":[{\"name\":\"小黄\"},{\"name\":\"小红\"}]}","$.contact",JSON_OBJECT("asd",1))
```
```sql
select JSON_REPLACE("{\"contact\":[{\"name\":\"小黄\"},{\"name\":\"小红\"}]}","$.contact","$.contact[0].name","小蓝")
```
[JSON Path Syntax](https://dev.mysql.com/doc/refman/5.7/en/json.html)
[JSON Function Reference](https://dev.mysql.com/doc/refman/5.7/en/json-function-reference.html)

# 安装前的清理工作
1. 清理原有mysql数据库
```bash
rpm -qa | grep mysql

#显示结果
#mysql80-community-release-el7-1.noarch
#mysql-community-server-8.0.11-1.el7.x86_64
#...

#使用以下命令依次删
yum remove mysql-xxx

```

2. 删除mysql的配置文件，卸载不会自动删除配置文件
```bash
find / -name mysql
#根据需求，依次删
rm -rf /var/lib/mysql
```

3. 删除MariaDB的文件
```bash
rpm -qa | grep mariadb
#centos自带有mariadb-libs，可删可不删
rpm -e mariadb-libs-xxx
#检测到依赖会删除失败，强制删除
rpm -e --nodeps mariadb-libs-xxx
```

# 安装

1. 下载rpm包  
https://dev.mysql.com/downloads/repo/yum/  
```bash
wget https://repo.mysql.com//mysql80-community-release-el7-6.noarch.rpm 
```

2. 安装 yum repo文件并更新 yum 缓存
```bash
rpm -ivh mysql80-community-release-el7-6.noarch.rpm 
```
执行后，会在/etc/yum.repos.d/目录下生成mysql专属repo文件  
查看mysql yum仓库中mysql版本列表，和安装的版本
```bash
yum repolist all | grep mysql
```
若要修改安装的mysql版本，如下
```bash
yum-config-manager --disable mysql80-community
yum-config-manager --enable mysql57-community
```
或者编辑mysql repo文件，修改对应的enabled，0禁用 1启用

3. yum安装mysql
```java
yum install mysql-community-server
``` 

>安装mysql8可能遇到错误：Couldn’t open file /etc/pki/rpm-gpg/RPM-GPG-KEY-mysql-2022，执行以下命令
>`rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022`

4. 启动服务
启动mysql   
`service mysqld start`  
`service start mysqld.service`  
`systemctl start mysqld`  
设置为开机启动  
`systemctl enable mysqld`  

5. 使用（mysql8） 
查看初始密码
```bash
sudo grep 'temporary password' /var/log/mysqld.log
#A temporary password is generated for root@localhost: 6(HwM6_y??Zj
```

登录

```
mysql -uroot -p6(HwM6_y??Zj
```

修改密码，密码太简单会报错

```bash
alter user user() identified by 'An29_4&6ah+=';
```

远程连接

```bash
use mysql;
select host,user,authentication_string,plugin from user;
update user set host='%' where user = 'root';

#刷新权限
flush privileges;
```

安装后的my.cnf在/etc下

# 设置主从
编辑my.cnf
```bash
server_id=1 #集群中每台机的唯一标识
log_bin=/var/log/mysql/mysql-bin.log #要将/var/log/mysql的拥有者设为mysql:mysql
	#chown mysql:mysql -R /var/log/mysql
# gtid_mode=ON
# enforce_gtid_consistency=1
# log_bin_trust_function_creators=1 # 默认为0，不设置为1，则不能创建function



#仅在从机中设置，忽略同步的表
replicate-wild-ignore-table=mysql.*
replicate-wild-ignore-table=sys.*



```

>从机专属账户（也可以直接用root）
>\>grant replication slave on *.* to 'rep'@'10.0.0.3' identified by '123456'
>\>flush privileges;

- 配置同步参数

```bash
mysql> change master to
    -> master_host='10.12.58.7',
    -> master_user='root',
    -> master_password='Z%(OSinLCi!sk*X3h#MYSQL',
    -> master_port=3306,
    -> master_log_file='mysql-bin.000001',
    -> master_log_pos=154;
```

执行change master 命令后的信息保存在/var/lib/mysql下的master.info和relay-log.info

```bash
ll /var/lib/mysql/*info
```

- 启动主从同步进程
```bash
# 启动主从同步进程
mysql> start slave;
# 检查状态
mysql> show slave status \G
	# Slave_IO_Running: Yes
	# Slave_SQL_Running: Yes

```
