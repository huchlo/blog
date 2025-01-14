\#/usr/local/nginx/sbin/nginx -t                   \#测试nginx配置正确性
\#/usr/local/nginx/sbin/nginx                     \#启动Nginx
\#/usr/local/nginx/sbin/nginx -s reload            # 重新载入配置文件
\#/usr/local/nginx/sbin/nginx -s reopen            # 重启 Nginx
\#/usr/local/nginx/sbin/nginx -s stop              # 停止 Nginx

nginx -t -c nginx.conf
nginx -c nginx.conf


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

- # proxy_pass 




# nginx模块
查看安装的模块 `nginx -V`
```bash
configure arguments: --with-cc=cl --builddir=objs.msvc8 --with-debug --prefix= --conf-path=conf/nginx.conf --pid-path=logs/nginx.pid --http-log-path=logs/access.log --error-log-path=logs/error.log --sbin-path=nginx.exe --http-client-body-temp-path=temp/client_body_temp --http-proxy-temp-path=temp/proxy_temp --http-fastcgi-temp-path=temp/fastcgi_temp --http-scgi-temp-path=temp/scgi_temp --http-uwsgi-temp-path=temp/uwsgi_temp --with-cc-opt=-DFD_SETSIZE=1024 --with-pcre=objs.msvc8/lib/pcre2-10.39 --with-zlib=objs.msvc8/lib/zlib-1.3.1 --with-http_v2_module --with-http_realip_module --with-http_addition_module --with-http_sub_module --with-http_dav_module --with-http_stub_status_module --with-http_flv_module --with-http_mp4_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_auth_request_module --with-http_random_index_module --with-http_secure_link_module --with-http_slice_module --with-mail --with-stream --with-stream_realip_module --with-stream_ssl_preread_module --with-openssl=objs.msvc8/lib/openssl-3.0.14 --with-openssl-opt='no-asm no-tests -D_WIN32_WINNT=0x0501' --with-http_ssl_module --with-mail_ssl_module --with-stream_ssl_module
```

## 开启https
在nginx的安装目录执行
```bash
./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
make
#不执行 make install
```
如果报错 ./configure: error: SSL modules require the OpenSSL library.则需要安装openssl模块
`yum -y install openssl openssl-devel`

## lua脚本

```bash
# 下载LuaJIT
wget https://github.com/LuaJIT/LuaJIT/archive/refs/tags/v2.1.ROLLING.tar.gz
# 下载ngx_devel_kit
wget https://github.com/vision5/ngx_devel_kit/archive/refs/tags/v0.3.1.tar.gz
# 下载lua-nginx-module v0.10.27对应nginx1.27 同理 v0.10.20对应nginx1.20
wget https://github.com/openresty/lua-nginx-module/archive/refs/tags/v0.10.27.tar.gz
wget https://github.com/openresty/lua-nginx-module/archive/refs/tags/v0.10.20.tar.gz
# \lib\resty\core\base.lua 查看对应lua-nginx-module版本
wget https://github.com/openresty/lua-resty-core/archive/refs/tags/v0.1.22.tar.gz
wget https://github.com/openresty/lua-resty-lrucache/archive/refs/tags/v0.15.tar.gz

tar -xzvf  xxx.tar.gz

cd LuaJIT-2.1.ROLLING
make PREFIX=/usr/local/luajit install
ln -sf /usr/local/luajit/bin/luajit-2.1. /usr/local/bin/luajit
luajit -v

echo "export LUAJIT_LIB=/usr/local/luajit/lib" >> /etc/profile
echo "export LUAJIT_INC=/usr/local/luajit/include/luajit-2.1" >> /etc/profile
source /etc/profile

ln -sf /usr/local/luajit/lib/libluajit-5.1.so.2 /usr/local/lib/libluajit-5.1.so.2

cd nginx-1.26.0  # 假设nginx源码位于nginx-1.26.0文件夹
./configure ... --add-module=/root/install/web_spec_ini/nginx-1.20.1/lua_module/ngx_devel_kit-0.3.1 --add-module=/root/install/web_spec_ini/nginx-1.20.1/lua_module/lua-nginx-module-0.10.20
--add-dynamic-module=/root/install/web_spec_ini/nginx-1.20.1/lua_module/lua-nginx-module-0.10.20 
#1 修改ld.so.conf就不用添加这个选项
--with-ld-opt="-Wl,-rpath,/usr/local/luajit/lib,--as-needed,-O1,--sort-common" 
#2
echo "/usr/local/luajit/lib" >> /etc/ld.so.conf
ldconfig

ldd $(which nginx)
nginx -V

lua_package_path '/root/install/web_spec_ini/nginx-1.20.1/lua_module/lua-resty-core-0.1.22/lib/?.lua;/root/install/web_spec_ini/nginx-1.20.1/lua_module/lua-resty-lrucache-0.15/lib/?.lua;';

set_by_lua_block $first_char {
	return "123"
}
add_header X-Last-Part $first_char;

content_by_lua_block {
	ngx.say("Hello from Lua!");
}


```