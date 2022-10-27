download page： https://my.visualstudio.com/Benefits?Wt.mc_id=o~msft~sql-server-dev-edition&%3bcampaign=o~msft~sql-server-dev-edition
126.com + qwer1025

Microsoft SQLServer 2014 版本区别http://blog.sina.com.cn/s/blog_671c54fe0102vnk0.html

注意事项：
SQL SERVER 2014 安装完成以后，不像SQL SERVER 2008 R2 会提供一个 BIDS 开发工具，也不像 SQL SERVER 2012 会提供一个 SSDT 开发工具
也就是说 BI 的开发工具（SSIS, SSRS, SSAS）在安装完 SQL SERVER 2014 之后是没有的，需要单独的安装。

sqlserver 2000-2012各版本下载 http://www.cnblogs.com/chixiaojin/p/3500846.html
中文下载页面：http://www.imsdn.cn/servers/sql-server-2014/

Apache Tomcat专辑 官方原版 http://download.csdn.net/album/detail/3609
和平之翼代码生成器合集 http://download.csdn.net/album/detail/3569
 

Oracle中有sequence的功能，SQL Server类似的功能使用Identity列实现，但是有很大的局限性。在2012中，微软终于增加了 sequence 对象，功能和性能都有了很大的提高。

```sql
INSERT INTO dbo.Examp1(Seq,Name)VALUES(NEXTVALUEFORdbo.Seq,'Tom');
```

```sql
Select CONVERT(varchar(100), GETDATE(), 23): 2006-05-16 
Select CONVERT(varchar(100), GETDATE(), 0): 05 16 2006 10:57AM 
Select CONVERT(varchar(100), GETDATE(), 1): 05/16/06 
Select CONVERT(varchar(100), GETDATE(), 2): 06.05.16 
Select CONVERT(varchar(100), GETDATE(), 3): 16/05/06 
Select CONVERT(varchar(100), GETDATE(), 4): 16.05.06 
Select CONVERT(varchar(100), GETDATE(), 5): 16-05-06 
Select CONVERT(varchar(100), GETDATE(), 6): 16 05 06 
Select CONVERT(varchar(100), GETDATE(), 7): 05 16, 06 
Select CONVERT(varchar(100), GETDATE(), 8): 10:57:46 
Select CONVERT(varchar(100), GETDATE(), 9): 05 16 2006 10:57:46:827AM 
Select CONVERT(varchar(100), GETDATE(), 10): 05-16-06 
Select CONVERT(varchar(100), GETDATE(), 11): 06/05/16 
Select CONVERT(varchar(100), GETDATE(), 12): 060516 
Select CONVERT(varchar(100), GETDATE(), 13): 16 05 2006 10:57:46:937 
Select CONVERT(varchar(100), GETDATE(), 14): 10:57:46:967 
Select CONVERT(varchar(100), GETDATE(), 20): 2006-05-16 10:57:47 
Select CONVERT(varchar(100), GETDATE(), 21): 2006-05-16 10:57:47.157 
Select CONVERT(varchar(100), GETDATE(), 22): 05/16/06 10:57:47 AM 
Select CONVERT(varchar(100), GETDATE(), 23): 2006-05-16 
Select CONVERT(varchar(100), GETDATE(), 24): 10:57:47 
Select CONVERT(varchar(100), GETDATE(), 25): 2006-05-16 10:57:47.250 
Select CONVERT(varchar(100), GETDATE(), 100): 05 16 2006 10:57AM 
Select CONVERT(varchar(100), GETDATE(), 101): 05/16/2006 
Select CONVERT(varchar(100), GETDATE(), 102): 2006.05.16 
Select CONVERT(varchar(100), GETDATE(), 103): 16/05/2006 
Select CONVERT(varchar(100), GETDATE(), 104): 16.05.2006 
Select CONVERT(varchar(100), GETDATE(), 105): 16-05-2006 
Select CONVERT(varchar(100), GETDATE(), 106): 16 05 2006 
Select CONVERT(varchar(100), GETDATE(), 107): 05 16, 2006 
Select CONVERT(varchar(100), GETDATE(), 108): 10:57:49 
Select CONVERT(varchar(100), GETDATE(), 109): 05 16 2006 10:57:49:437AM 
Select CONVERT(varchar(100), GETDATE(), 110): 05-16-2006 
Select CONVERT(varchar(100), GETDATE(), 111): 2006/05/16 
Select CONVERT(varchar(100), GETDATE(), 112): 20060516 
Select CONVERT(varchar(100), GETDATE(), 113): 16 05 2006 10:57:49:513 
Select CONVERT(varchar(100), GETDATE(), 114): 10:57:49:547 
Select CONVERT(varchar(100), GETDATE(), 120): 2006-05-16 10:57:49 
Select CONVERT(varchar(100), GETDATE(), 121): 2006-05-16 10:57:49.700 
Select CONVERT(varchar(100), GETDATE(), 126): 2006-05-16T10:57:49.827 
Select CONVERT(varchar(100), GETDATE(), 130): 18 ???? ?????? 1427 10:57:49:907AM 
Select CONVERT(varchar(100), GETDATE(), 131): 18/04/1427 10:57:49:920AM
```