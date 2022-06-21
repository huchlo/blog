多线程

数据格式

file io

spring

[Jackson使用手册 (altitude.xin)](https://www.altitude.xin/blog/home/#/chapter/60a4fe28746efc4e234e3724ece916c6?id=%F0%9F%A5%AD-jackson%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C)

https://www.injdk.cn/

xxx/em-sw/dev/master
xxx/config name/文件后缀/分支

# mybatis
REQUIRED 必须的
```xml
<!ELEMENT select (#PCDATA | include | trim | where | set | foreach | choose | if | bind)*>
<!ATTLIST select
id CDATA #REQUIRED  唯一标识符
parameterMap CDATA #IMPLIED  弃用
parameterType CDATA #IMPLIED  传入参数的类
resultMap CDATA #IMPLIED  返回结果的映射
resultType CDATA #IMPLIED  返回结果的类，如果返回的是集合，则应设为集合包含的类型
resultSetType (FORWARD_ONLY | SCROLL_INSENSITIVE | SCROLL_SENSITIVE) #IMPLIED
statementType (STATEMENT|PREPARED|CALLABLE) #IMPLIED
fetchSize CDATA #IMPLIED
timeout CDATA #IMPLIED
flushCache (true|false) #IMPLIED
useCache (true|false) #IMPLIED 
databaseId CDATA #IMPLIED
lang CDATA #IMPLIED
resultOrdered (true|false) #IMPLIED
resultSets CDATA #IMPLIED
>
```
choose、when、otherwise
when就相当于if，otherwise就相当于else，但是when、otherwise需要放在choose里面才能使用

```xml
<choose> 
        <when test="list != null and list.size() > 0"> xxx </when>
        <otherwise>XXX</otherwise> 
</choose>
```
