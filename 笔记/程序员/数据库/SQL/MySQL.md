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

## varchar长度
长度<=64时，取2^n，varchar(8)、varchar(16)、varchar(32)、varchar(64)
当长度>64时，取2^n-1，varchar(127)、varchar(255)、varchar(511)...

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

# 时间相关
- timestampdiff（unit，begin，end）
unit为单位，如second、minute、hour等，(end-begin)的时间差

# 函数
```sql
CREATE DEFINER=`root`@`%` FUNCTION `函数名`(`入参` varchar(15)) RETURNS tinyint(4)
BEGIN
DECLARE vs tinyint DEFAULT 0;
select LENGTH(入参) into vs；
RETURN vs;
END
```

# 存储过程
参数模式，in、inout、out，in是入参，out是出参（默认最后会select一下所有out参数）
```sql
CREATE DEFINER=`root`@`%` PROCEDURE `过程名`(IN `aa` tinyint,INOUT `ss` tinyint,OUT `dd` tinyint)
BEGIN
	select 1;
END
```
