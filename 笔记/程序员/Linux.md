# 基础认识

> :/$ 指系统的根目录，快速进入方式：`cd /.`
> :~$指用户的根目录，一般在系统的home/username中，快速进入方式：`cd ~/`
> 执行\*.sh文件:  `sh *.sh./*.sh`
> 发放权限 `chmod +x filename`

# ssh免密登录
- 先生成密钥，不然会报错
ssh-keygen -t rsa
ssh-keygen -t dsa
- 把本地主机的公钥复制到远程主机的authorized_keys文件上
ssh-copy-id root@com01
需要输入com01的密码
- 本机ssh免密
ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
http://lnmp.ailinux.net/ssh-keygen

# 常用命令
[Linux命令学习手册](http://lnmp.ailinux.net/)
[Linux命令大全 | 菜鸟教程](https://www.runoob.com/linux/linux-command-manual.html)

```bash
& #表示后台执行
&& #表示前一条命令执行成功，才执行下一条命令 echo '1‘ && echo '2'
| #表示管道，上一条命令的输出作为下一条命令的参数
|| #上一条命令失败后才执行下一条

pwd cd ls 
mkdir -m 700 /root/test #创建目录
cp -a aaa/* /bbb #复制文件/目录，及子文件，a=dpr
mv /usr/men/* . #移动文件，重命名文件
rm -r * #递归删除, -f 不询问
chmod 764 /root/test #设置权限
vi vim filename i :wq :q! :w! #文本编辑器
touch filename1 filename2 #创建文件，更新文件时间标签

more filename #按页显示文本文件的内容，替换cat
head -c 6 filename #显示头部内容
tail -f filename #查看尾部内容，-f 显示最新追加的内容

ps -ef|grep xxx #查看进程，grep xxx 把匹配的行打印出来
kill pid #默认kill -15，通知进程自行结束
kill -3 pid #中断，并打印堆栈信息/proc/${pid}/cwd/antBuilderOutput.log
kill -9 pid #强制中断进程

netstat -tunlp |grep pid #查看端口，网络状态

history #显示历史命令

source /etc/profile #重新执行初始化文件，使之立即生效，

reboot #重启
halt #关机，会判断执行shutdown
poweroff #关机后断电
logout #退出当前登录的shell

nohup # 将程序以忽略挂起信号的方式运行,被运行的程序的输出信息将不会显示到终端
nohup java -jar xxx >/dev/null 2>&1 &
```

## 查看系统相关命令

```bash
cat /proc/cpuinfo |grep "model name" && cat /proc/cpuinfo |grep "physical id" # CPU大小
cat /proc/meminfo |grep MemTotal # 内存大小
fdisk -l |grep Disk # 硬盘大小 
uname -a # 查看内核/操作系统/CPU信息的linux系统信息命令
head -n 1 /etc/issue # 查看操作系统版本，是数字1不是字母L
cat /proc/cpuinfo # 查看CPU信息的linux系统信息命令
hostname # 查看计算机名的linux系统信息命令
lspci -tv # 列出所有PCI设备
lsusb -tv # 列出所有USB设备的linux系统信息命令
lsmod # 列出加载的内核模块
env # 查看环境变量资源
free -m # 查看内存使用量和交换区使用量
df -h # 查看各分区使用情况
du -sh # 查看指定目录的大小 
grep MemTotal /proc/meminfo # 查看内存总量
grep MemFree /proc/meminfo # 查看空闲内存量
uptime # 查看系统运行时间、用户数、负载
cat /proc/loadavg # 查看系统负载磁盘和分区
mount | column -t # 查看挂接的分区状态 
fdisk -l # 查看所有分区
swapon -s # 查看所有交换分区
hdparm -i /dev/hda # 查看磁盘参数(仅适用于IDE设备) 
dmesg | grep IDE # 查看启动时IDE设备检测状况网络 
ifconfig # 查看所有网络接口的属性
iptables -L # 查看防火墙设置 
route -n # 查看路由表
netstat -lntp # 查看所有监听端口
netstat -antp # 查看所有已经建立的连接
netstat -s # 查看网络统计信息进程
ps -ef # 查看所有进程
top # 实时显示进程状态用户
w # 查看活动用户
id # 查看指定用户信息
last # 查看用户登录日志
cat /etc/passwd # 查看系统所有用户
cat /etc/group # 查看系统所有组
crontab -l # 查看当前用户的计划任务服务
chkconfig –list # 列出所有系统服务
chkconfig –list | grep on # 列出所有启动的系统服务程序
rpm -qa # 查看所有安装的软件包
cat /proc/cpuinfo ：查看CPU相关参数的linux系统命令
cat /proc/partitions ：查看linux硬盘和分区信息的系统信息命令
cat /proc/meminfo ：查看linux系统内存信息的linux系统命令
cat /proc/version ：查看版本，类似uname -r
cat /proc/ioports ：查看设备io端口
cat /proc/interrupts ：查看中断
cat /proc/pci ：查看pci设备的信息
cat /proc/swaps ：查看所有swap分区的信息
```


## 命令工具
### `tar` `zip` `unzip` `bzip2` `bunzip2` `gunzip` `gzip`
```bash
tar -xvf FileName.tar
tar -xzvf  FileName.tar.gz
tar -jxvf FileName.tar.bz2
```
[tar命令打包解压示例 - Linux命令大全教程™ (yiibai.com)](https://www.yiibai.com/linux/tar.html)
### `yum`
```bash
yum list
yum search <keyword>
yum info <package_name>
yum install <package_name>
yum update <package_name>
yum remove <package_name>
```
- 切换yum源
即替换/etc/yum.repos.d/CentOS-Base.repo文件
[CentOS 源使用帮助 — USTC Mirror Help 文档](https://mirrors.ustc.edu.cn/help/centos.html)
替换之后记得 `yum makecache`
### `systemd`
在systemd中，所有的服务脚本都称为unit，主要分成6类：.service, .socket, .target, .path, snapshot, .timer，它们都存放在/usr/lib/systemd/system/目录中
#systemctl
systemctl是 Systemd 的主命令，用于管理系统，统一采用systemctl命令来管理所有的服务
```
systemctl [start|stop|restart|reload|status|is-active|is-enable|enable|disable|mask|umask] 服务名.类型
```
当服务为service类型时可以省略类型，如atd.service可以简写为atd，其他的类型不能省略，如talnet.socket
systemclt set-default|get-default|isolate xxxxx.target 设置默认运行级别|获取当前的默认运行级别|不重启切换当前环境  （什么是环境呢，target类型的服务都为环境，当运行或切换（需要使用isolate而不能使用start）一个环境时往往会伴随着启动很多其他的服务用以支持这个环境，最常见的环境就是字符界面和图形界面，比如想从现在的字符界面临时切换到图形界面，使用systemctl isolate graphical.tatget）
在systemd中，运行级别由/etc/systemd/system/default.target定义，这个文件本身是一个软连接，如果它指向graphical.targer那么默认的运行级别就是图形界面。
在/usr/lib/systemd/system/目录中，环境的变化往往会伴随着很多其他服务，而wants目录就是当target类型的服务切换之后自动运行的其他服务。
```bash
systemctl #列出所有已启动的服务
systemctl list-units #同上
systemctl list-units --all #列出所有服务，包括没启动的
systemctl list-unit-files #列出/usr/lib/systemd/system/目录内所有的服务文件
systemctl list-units --type=service --all #列出所有service类型的服务，其中--type的取值还可以是target,socket等等
systemctl list-units --type=service --all|grep -i cpu #列出所有和cpu相关的服务

systemctl poweroff #关机（相当于systemctl isolate poweroff.target）
systemctl reboot #重启
systemctl suspend #暂停/睡眠，将系统数据写入内存，同时将大部分硬件关闭，等待唤醒（相当于Windows下的睡眠） 
systemctl hibernate #休眠，将系统数据写入硬盘，然后关机

systemctl rescue 进入救援模式

systemctl emergency 进入紧急模式，比救援模式更强更彻底
```
#systemd-analyze
`systemd-analyze`命令用于查看启动耗时。
```bash
# 查看启动耗时
$ systemd-analyze                                                                                       
# 查看每个服务的启动耗时
$ systemd-analyze blame
# 显示瀑布状的启动过程流
$ systemd-analyze critical-chain
# 显示指定服务的启动流
$ systemd-analyze critical-chain atd.service
```
#hostnamectl
`hostnamectl`命令用于查看当前主机的信息。
```bash
# 显示当前主机的信息
$ hostnamectl
# 设置主机名。
$ sudo hostnamectl set-hostname rhel7
```
#localectl
`localectl`命令用于查看本地化设置。
```bash
# 查看本地化设置
$ localectl
# 设置本地化参数。
$ sudo localectl set-locale LANG=en_GB.utf8
$ sudo localectl set-keymap en_GB
```
#timedatectl
`timedatectl`命令用于查看当前时区设置。
```bash
# 查看当前时区设置
$ timedatectl
# 显示所有可用的时区
$ timedatectl list-timezones                                                                                   
# 设置当前时区
$ sudo timedatectl set-timezone America/New_York
$ sudo timedatectl set-time YYYY-MM-DD
$ sudo timedatectl set-time HH:MM:SS
```
#loginctl
`loginctl`命令用于查看当前登录的用户。
```bash
# 列出当前session
$ loginctl list-sessions
# 列出当前登录用户
$ loginctl list-users
# 列出显示指定用户的信息
$ loginctl show-user ruanyf
```

### firewalld
```bash
systemctl status firewalld #查看防火墙状态
systemctl is-enabled firewalld #查看防火墙是否开机启动
systemctl start firewalld #启动
systemctl stop firewalld #关闭防火墙
systemctl disable firewalld #开机禁用
systemctl enable firewalld #开机启用

firewall-cmd --state #查看默认防火墙状态
firewall-cmd --permanent --add-rich-rule='rule protocol value=icmp drop' #禁止被ping（丢弃ICMP包）
```
https://firewalld.org/documentation/man-pages/firewall-cmd.html

### `iptables`
```bash
iptables -L -n -v #查看已添加的iptables规则
```

# 源码安装3步骤

```bash
./configure
```
检测安装平台的目标特征，生成 Makefile
```bash
make
```
从Makefile中读取指令，然后编译
```bash
make install
```
从Makefile中读取指令，然后安装到指定的位置，运行这个要有足够的权限
>./configure --help
在`configure`后加上参数来对安装进行控制
`make`过程中出现错误，一般是系统缺少依赖库
`make insatll`安装需要有root权限，正式安装之前，可以`make check`或`make test`来进行测试，安装完成之后，可以`make clean`清除编译产生的文件

# 修改主机名字
```bash
hostname #查看
hostnamectl set-hostname centos01 # 修改法一，使用这个命令会立即生效且重启也生效
hostname centos02 # 修改法二，立即生效,不过重启后失效
vi /etc/hostname # 修改法三，重启后生效
centos03

vim /etc/sysconfig/network #HOSTNAME属性上尽量设置同/etc/hostname的一致
NETWORKING=yes
HOSTNAME=centos03
# /etc/hosts存放的是域名与ip的对应关系
```

# Shell脚本
shell解释器：`/bin/sh` `/bin/bash`，bash是sh的增强版本，文件后缀名都是 sh。
第一行必须为“#！/bin/bash”，脚本声明(#!)用来告诉系统使用哪种Shell解释器来执行该脚本。
第一行以后可以添加注释信息（#）对脚本功能和某些命令的介绍信息，使得自己或他人在日后看到这个脚本内容时，可以快速知道该脚本的作用或一些警告信息。
```bash
＃!/bin/bash
echo Hello World! # 输出：Hello World!
var1="变量" # ''会使变量无效，无引号和""可以有变量
for var1 in `ls /etc`
echo $var1 # 使用变量
readonly var1 # 设置变量只读
unset var1 # 删除变量
echo ${#var1} # 字符串变量长度
echo ${var1:1:4} # 截取字符串
echo `expr index "$var1" xxx` # 查找子字符串的索引
var1=('数组元素1' '数组元素2' '数组元素3')
var[1] = '数组元素'
echo ${var1[n]} # 读取数组，${var1}相当于${var[0]}
echo ${#var1[*]} # 数组长度，${#var1[@]}
echo ${#var1[n]} # 数组元素长度
echo `date` # 显示当前日期
echo $n # n 代表一个数字，1为第一个参数，2为第二个参数...,$0为执行的文件名
echo $# # 传递到脚本的参数个数
echo "$*" # 以单字符串输出所有参数，不加"则以数组输出
echo $@ # 加不加"都以数组字符串输出所有参数
echo $$ # 脚本运行的进程ID号
echo $! # 后台运行的最后一个进程号
echo $- # shell使用的选项，set
echo $? # 显示命令退出的状态，0表示没有错误

echo -e "OK! \c" # -e 开启转义 \c 不换行
echo -n '请输入: ' # 同上，不换行

echo `expr 1+2` # 算数运算，+ - * / % 
if [$a -eq $b] # 关系运算，== != -eq -ne -gt -lt -ge -le
if [xx && xx] # 逻辑运算，&& || ! -o -a
if [$a = $b] # 字符串运算，= != -z(长度是否为0) -n(长度是否不为0) $
file="/usr/local/bin/test"
if [ -r $file ] # 文件是否可读
if [ -w $file ] # 文件是否可写
if [ -x $file ] # 文件是否可执行
if [ -f $file ] # 文件是否为普通文件
if [ -d $file ] # 文件是否为一个目录
if [ -s $file ] # 文件是否不为空
if [ -e $file ] # 文件是否存在
if test -e ./xxx # 等同于if[-e ./xxx]

if[xxx] # if[]; then ...; fi 
then
	...
elif[ccc]
then	
	...
else
	...
fi

for loop in 1 2 3 # break continue
do
	echo $loop
done

while(cond) # 无限循环while true，for (( ; ; ))
do
	...
done

echo -n '请输入(CTRL+D退出): '  
while read var1  
do  
    echo "你输入了$var1"  
done

until cond # 直到cond为true中止循环
do
    command
done

case $var1 in
"start")
    command1
    ;;
"stop")
    command2
    ;;
esac

demoFun(){  # 定义函数
    echo "这是我的第一个 shell 函数!"  
    echo $1
    echo $2
    echo $3
    
}
demoFun 1 2 3# 执行函数

. filename # 执行另一个脚本,source filename

https://www.runoob.com/linux/linux-shell-io-redirections.html

chmod u+x \*.sh # 提示权限不足，需要给脚本文件增加执行权限即可
```


# VMware Workstation 虚拟机网络配置

Windows安装VMware Workstation后，会在网络连接设置中默认添加2个虚拟网络适配器：VMnet1、VMnet8

> 虚拟机网络适配器有3种模式
>
> - 仅主机模式：主机作为网关，主机与虚拟机共享专用网络（不连接公网）
> - NAT模式：主机作为网关，既有专用网络，虚拟机也可通过主机IP上网
> - 桥接模式：直接连接物理网络，可获取与主机同级的IP地址

VMnet1：仅主机模式，VMnet8：NAT模式
编辑 - 虚拟网络编辑器 设置子网IP和子网掩码

> 设置 10.0.0.0 255.0.0.0
> 网关默认为10.0.0.1
> 可将虚拟机的IP设置成10.0.0.2 10.0.0.3等
- 在centos7中设置静态IP（非必要）

> 修改对应网关设备的配置 `vi /etc/sysconfig/network-scripts/ifcfg-ens33`
> 修改参数
> BOOTPROTO = static （static静态，dhcp动态，none不指定）
> ONBOOT = yes （激活网卡）
> IPADDR = 10.0.0.2
> NETMASK = 255.0.0.0
> GATEWAY = 10.0.0.1
> DNS1 = 119.29.29.29

- 问题：使用ssh连接centos7虚拟机过慢
  分析：当通过SSH连接服务器时，服务器端会进行DNS检测，但是当环境在本地，这种检测很浪费时间

> `vi /etc/ssh/sshd_config`
> 把 UseDNS 改成 no
> `systemctl restart sshd`



