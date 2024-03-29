# 事务隔离级别

数据库事务的隔离级别有4个，由低到高依次为Read uncommitted 、Read committed 、Repeatable read 、Serializable ，后面三个级别可以逐个解决脏读 不可重复读 、幻读 这几类问题。
```sql
#查询当前会话的事务隔离级别
SELECT @@SESSION.tx_isolation
#设置当前客户端的隔离级别
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
```
- 脏读 dirty read

| 时间 | 转账业务                   | 取款业务                   |
| ---- | -------------------------- | -------------------------- |
| T1   |                            | 开始事务                   |
| T2   | 开始事务                   |                            |
| T3   |                            | 查询账户余额为1000元       |
| T4   |                            | 取出500，把余额改为500元   |
| T5   | 查询账余额为500元（脏读）  |                            |
| T6   |                            | 撤销事务，余额恢复为1000元 |
| T7   | 汇入100元，把余额改为600元 |                            |
| T8   | 提交事务                   |                            |

感觉就脏读对业务的影响大，不可重复读和幻读没什么影响，但是可以借着来理解数据库的事务隔离和锁
- 不可重复读

| 时间 | 统计金额业务 | 转账业务 |
| --- | --- | ---|
| T1 | |开始事务|
|T2|开始事务|
|T3|统计总存款为1000元||
|T3||查询账户余额为1000元|
|T4||取出500，把余额改为500元|
|T5||提交事务|
|T6|再次统计总存款为500元（不可重复读）||
|T7|事务结束||

- 幻读 

| 时间 | 统计金额业务 | 转账业务 |
| --- | --- | ---|
| T1 | |开始事务|
|T2|开始事务|
|T3|统计总存款为1000元||
|T4||新增一个存款账户，存款为100元|
|T5||提交事务|
|T6|再次统计总存款为1100元（幻读）||
|T7|事务结束||


不可重复读和幻读的区别是：不可重复读是指读到了已经提交的事务的更改数据(update)，幻读是指读到了其他已经提交事务的新增数据(insert或delete)。对于这两种问题解决采用不同的办法，防止读到更改数据，只需对操作的数据添加行级锁，防止操作中的数据发生变化;二防止读到新增数据，往往需要添加表级锁，将整张表锁定，防止新增数据(oracle采用多版本数据的方式实现)。

```sql
select

(SELECT EXISTS(SELECT 1 FROM t_smartcare WHERE imei = t_device_wm.imei LIMIT 1)) as smartStatus,

IF(expr1,expr2,expr3) as zxc, --如果expr1为true，返回expr2，否则expr3

IFNULL(expr1,expr2) as asd, --如果expr1不为null则返回expr1，否则expr2



from
```

# 方法
获取当前日期
`SELECT current_date();`
`# 2023-01-09`

字符串替换
`SELECT REPLACE(current_date(),'-','');`
`20230109`

查询后插入
`INSERT INTO table (sn,p1,p2) select sn,'qq','tt' from table2;`