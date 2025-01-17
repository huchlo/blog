官网： https://www.mysql.com/  
开发者文档： https://dev.mysql.com/doc/

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

# windows安装
https://dev.mysql.com/downloads/installer/ 新版可以直接安装了
解压缩
       将下载到的文件解压缩到自己喜欢的位置，例如我自己的位置是D:\Program Files\mysql-5.7.10-winx64
添加环境变量
       右键计算机->属性->高级系统设置->环境变量；在系统变量里添加MYSQL_HOME环境变量，变量值为MySQL的根目录，例如我的是D:\Program Files\mysql-5.7.10-winx64（原路径有错，已更改，对受误导的网友表示抱歉。谢谢网友“庞大进”的提醒，2016.5.7）
       找到path，选择编辑，在原有值末尾添加;%MYSQL_HOME%\bin
添加配置文件生成的初始密码，记下来（因为有特殊字符，很容易记错，最好把整个消息保存在记事本里）4TSrgw;tyPaW
如果上述命令运行不成功请用以下命令代替：
%MYSQL_HOME%\bin\mysqld --initialize --user=mysql --console
如果仍然不成功请检查第２步
       在MySQL的安装目录（例如我的是D:\Program Files\mysql-5.7.10-winx64）下，建立新文本文件txt，并将其命名为my.ini（注意扩展名也要修改）。
双击打开该文件，并在其中添加内容如下：
[mysqld]
basedir=D:\Program Files\mysql-5.7.10-winx64
datadir=D:\Program Files\mysql-5.7.10-winx64\data
port = 3306
保存后关闭
初始化数据库 
       以管理员自身份打开CMD执行以下命令（注意必须以管理员身份打开，否则报错）
mysqld --initialize --user=mysql --console
在控制台消息尾部会出现随机
将MySQL添加到系统服务
       以管理员自身份打开CMD执行以下命令（注意必须以管理员身份打开，否则报错）
mysqld --install MySQL
net start MySQL
安装成功，则显示“服务已启动成功”
如果上述命令运行不成功，可以用以下命令代替：
%MYSQL_HOME%\bin\mysqld --install MySQL
net start MySQL
(第２步改了之后，之前这里忘记了更改，谢谢网友穆novA的提醒，2016.6.12)
安装成功，则显示“服务已启动成功”
如果仍然不成功请检查第２步
启动MySQL并修改密码
       在CMD控制台里执行命令  mysql -u root -p
回车执行后，输入刚才记录的随机密码
执行成功后，控制台显示 mysql>，则表示进入mysql
输入命令set password for root@localhost = password('123'); （注意分号）
此时root用户的密码修改为123

```bash
C:\Windows\system32>mysqld --initialize --user=mysql --console
mysqld: Can't create directory 'D:\mysql-5.7.13\data\' (Errcode: 2 - No such file or directory)
2016-08-02T03:30:27.687838Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2016-08-02T03:30:27.859282Z 0 [ERROR] Can't find error-message file 'D:\mysql-5.7.13\share\errmsg.sys'. Check error-message file location and 'lc-messages-dir' configuration directive.
2016-08-02T03:30:28.904014Z 0 [ERROR] Aborting
```

服务列表里没有MySQL服务，故出现该错误。请进入MySQL的bin目录，并在bin目录打开命令行窗口，在命令行窗口输入：mysqld --install，回车，提示：Service successfully installed。表示安装MySQL服务成功，命令行窗口输入：net start mysql ，可以正常启动。

跳过权限检查启动MySQL，

D:/MySQL/MySQL Server 5.0/bin/mysqld-nt –skip-grant-tables

.重新打开一个CMD，进入D:/MySQL/MySQL Server 5.0/bin/，

重设root密码

D:/MySQL/MySQL Server 5.0/bin/mysqladmin -uroot flush-privileges password “newpassword”

D:/MySQL/MySQL Server 5.0/bin/mysqladmin -u root -p shutdown

将newpassword替换为你的新密码，第二个命令会让你重复输入一次新 密码。

update mysql.user set authentication_string=password('123456') where user='root' and Host = 'localhost';

删除服务
mysqld --remove MySQL（服务名）

mysql 远程登陆（不安全）
修改mysql-user user=root行的host，localhost修改为%，再重启数据库
