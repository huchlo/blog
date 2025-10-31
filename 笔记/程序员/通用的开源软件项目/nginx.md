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

# conf/nginx.conf
- 结构
```console
http{
	server{ 
		listen       80;
	    server_name  127.0.0.1;
        location / {
            root   html;
            index  index.html index.htm;
        }
        #资源
        location / {
        root C:\Users\longl\Videos;
        autoindex on;                                  
        autoindex_exact_size off;                
        autoindex_localtime on;  
        access_log   off;
        expires      2d;
        }
	    ...
 	}
	server{ 
		listen       80;
	    server_name  192.168.1.101;
	    ...
    }
    server{
	    listen       443 ssl;
	    server_name  localhost;
	    root html;
        ssl_certificate      cert/2756403_api.data-school.cn.pem;
        ssl_certificate_key  cert/2756403_api.data-school.cn.key;
        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;
        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers  on;
        location / {
            proxy_pass http://backServer/;
            index  index.html index.htm;
        }
    }
}
```

- 

# 开启https
在nginx的安装目录执行
```bash
./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
make
#不执行 make install,如果执行了会覆盖之前的安装
#刚编译好的nginx在./objs/nginx，需要cp ./objs/nginx /usr/local/nginx/sbin/
```
如果报错 ./configure: error: SSL modules require the OpenSSL library.则需要安装openssl模块
`yum -y install openssl openssl-devel`
