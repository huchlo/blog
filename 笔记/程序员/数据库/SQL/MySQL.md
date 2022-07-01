[MySQL](https://www.mysql.com/)  
[MySQL :: MySQL Documentation](https://dev.mysql.com/doc/)

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

# 安装