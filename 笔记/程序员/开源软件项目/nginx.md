\#/usr/local/nginx/sbin/nginx -t                   \#测试nginx配置正确性
\#/usr/local/nginx/sbin/nginx                     \#启动Nginx
\#/usr/local/nginx/sbin/nginx -s reload            # 重新载入配置文件
\#/usr/local/nginx/sbin/nginx -s reopen            # 重启 Nginx
\#/usr/local/nginx/sbin/nginx -s stop              # 停止 Nginx

# 安装
安装依赖
```bash
yum -y install gcc zlib zlib-devel pcre-devel openssl openssl-devel
```

```bash
#创建一个文件夹
cd /usr/local
mkdir nginx
cd nginx
#下载tar包
wget http://nginx.org/download/nginx-xxx.tar.gz
tar -xvf nginx-xxx.tar.gz
#进入nginx目录
cd /usr/local/nginx
#执行命令
./configure
#执行make命令
make
#执行make install命令
make install

```
# 基本认识
网页文件放在html
配置文件放在conf
命令在sbin/nginx

# 开启https
在nginx的安装目录执行
```bash
./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
make
#不执行 make install
```
如果报错 ./configure: error: SSL modules require the OpenSSL library.则需要安装openssl模块
`yum -y install openssl openssl-devel`
