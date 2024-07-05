## 表相关

```bash
select * from tableName limit 1\G
insert into tableName (name) values('a'),('b');
update tableName set name = 'haha';
delete from tableName [WHERE Clause]


show table status like 'tableName'; #表信息
show create table tableName; #表结构
show indexes from tableName from databaseName; #表索引信息

SELECT * FROM information_schema.Routines WHERE ROUTINE_SCHEMA = "databaseNanme"
SHOW CREATE FUNCTION functionName;
SHOW CREATE PROCEDURE procedureName;

ALTER TABLE tableName ADD COLUMN `asd` INT NULL DEFAULT '1' COMMENT 'zxc';
alter table t engine=InnoDB;
optimize table [$Database1].[Table1],[$Database2].[Table2]
```
## 基本信息

```bash
#mysql -uroot -p 进入之后
show databases; #显示所有数据库
use <数据库名>; #使用数据库
select database(); #打印当前使用的数据库
select version(); #打印数据库版本信息
select now(); #打印数据库当前时间
show variables like 'char%'; #数据库字符集

show tables; #显示当前数据库的所有表

SHOW TABLE STATUS FROM `db_gmweb_sgame`;
SHOW FUNCTION STATUS WHERE `Db`='db_gmweb_sgame';
SHOW PROCEDURE STATUS WHERE `Db`='db_gmweb_sgame';
SHOW TRIGGERS FROM `db_gmweb_sgame`;

```

# 备份与恢复
```bash
mysqldump -uroot -p [数据库名] > [文件]
mysqldump -uroot -p ss > /root/ss.db
#mysqldump -uroot -p'password' -h127.0.0.1 --all-databases > /root/ss.db
mysqldump -d -usgame -plook2022 db_gmweb_sgame >db_gmweb_sgame.sql
#-d表示只导出表结构
mysqldump -R --ignore-table=db_gmweb_sgame.game_news --ignore-table=db_gmweb_sgame.game_notice --ignore-table=db_gmweb_sgame.game_server --ignore-table=db_gmweb_sgame.game_sys_logininfor --ignore-table=db_gmweb_sgame.game_sys_oper_log --ignore-table=db_gmweb_sgame.gen_table --ignore-table=db_gmweb_sgame.gen_table_column --ignore-table=db_gmweb_sgame.look_admin_action_log --ignore-table=db_gmweb_sgame.look_admin_add_gold_coinlog --ignore-table=db_gmweb_sgame.look_parameter --ignore-table=db_gmweb_sgame.look_student --ignore-table=db_gmweb_sgame.qrtz_blob_triggers --ignore-table=db_gmweb_sgame.qrtz_calendars --ignore-table=db_gmweb_sgame.qrtz_cron_triggers --ignore-table=db_gmweb_sgame.qrtz_fired_triggers --ignore-table=db_gmweb_sgame.qrtz_job_details --ignore-table=db_gmweb_sgame.qrtz_locks --ignore-table=db_gmweb_sgame.qrtz_paused_trigger_grps --ignore-table=db_gmweb_sgame.qrtz_scheduler_state --ignore-table=db_gmweb_sgame.qrtz_simple_triggers --ignore-table=db_gmweb_sgame.qrtz_simprop_triggers --ignore-table=db_gmweb_sgame.qrtz_triggers --ignore-table=db_gmweb_sgame.stats_channel --ignore-table=db_gmweb_sgame.stats_platform --ignore-table=db_gmweb_sgame.stats_retained --ignore-table=db_gmweb_sgame.stats_retained_first_rech --ignore-table=db_gmweb_sgame.cd  --ignore-table=db_gmweb_sgame.stats_roi --ignore-table=db_gmweb_sgame.stats_roi_date --ignore-table=db_gmweb_sgame.stats_role --ignore-table=db_gmweb_sgame.stats_room --ignore-table=db_gmweb_sgame.stats_room_role --ignore-table=db_gmweb_sgame.t_deposit_order --ignore-table=db_gmweb_sgame.t_gold_log --ignore-table=db_gmweb_sgame.t_online_log --ignore-table=db_gmweb_sgame.t_rech_rtained_log --ignore-table=db_gmweb_sgame.t_role_agent --ignore-table=db_gmweb_sgame.t_role_agent_logt_role_simple --ignore-table=db_gmweb_sgame.t_rtained_log --ignore-table=db_gmweb_sgame.t_withdraw_order -h10.0.17.28 -usgame -plook2022 db_gmweb_sgame >db_gmweb_sgame_fun.sql
#-R表示导出表结构

#仅导出结构和函数
mysqldump -h172.31.16.192 -usgame -plook2022 --no-data --routines --events db_gmweb_sgame > backup.sql

mysqldump -h172.31.16.192 -usgame -plook2022 db_gmweb_sgame sys_config sys_dept sys_dict_data sys_dict_type sys_job sys_menu sys_post sys_role sys_role_dept sys_role_menu sys_user sys_user_post sys_user_role> backup2.sql

mysqldump -h10.0.17.28 -usgame -plook2022 db_gmweb_sgame sys_config sys_dept sys_dict_data sys_dict_type sys_job sys_menu sys_post sys_role sys_role_dept sys_role_menu sys_user sys_user_post sys_user_role> backup2.sql

#恢复
#数据库要先创建好
mysql -uroot -p ss_gansu_20221010 < /home/app/ss_gansu_20221010.db

mysql -u'root' -p'password'
mysql> create database abc; # 创建数据库 
mysql> use abc; # 使用已创建的数据库 
show variables like 'char%';
mysql> set names utf8; # 设置编码 
mysql> source /home/abc/abc.sql # 导入备份数据库
```

# 从主从同步错误恢复
```bash
主 show master status; #查看主库状态
从 show slave \G; #查看slave状态

#忽略错误，继续同步
##解析binlog，可以看到执行错误的sql语句
mysqlbinlog -v --stop-position=123 mysql-bin.000001 > /tmpbinlog.log

主 flush tables with read lock;
从 stop slave;
从 set global sql_slave_skip_counter=1; #跳过1次错误
从 start slave; #手动修改从库后，启用

#重新导入数据，重新同步
主 flush tables with read lock;
#主mysqldump导出，然后从source导入（如果是单库，先use，后source）
#mysqldump -uroot -p -hlocalhost --all-databases > mysql.db
#scp mysql.sql root@10.6.97.134:/tmp/
从 stop slave；
从 source
从 change #查询主库的status，之前可以reset slave;或reset slave all;一下 
从 start slave;
主 unlock tables;
```