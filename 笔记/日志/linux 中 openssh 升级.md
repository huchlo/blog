./configure
make
make install


` yum install gcc`

` yum install zlib zlib-devel`

```bash
 configure: error: *** OpenSSL headers missing - please install first or check config.log ***
yum install -y openssl-devel
```

 源码编译安装后，版本没升级

# ssh -V

-bash: /usr/local/bin/ssh: No such file or directory

出现这个报错只能使用CD命令-bash 命令路径 “No such file or directory”

profile配置文件出错导致

解决方法：

export PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin

 source /etc/profile

