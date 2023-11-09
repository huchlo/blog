免费https证书
[Let's Encrypt (letsencrypt.org)](https://letsencrypt.org/)

通过[Certbot (eff.org)](https://certbot.eff.org/)创建

以centos7 nginx为例

先按官方教程走，安装`certbot`

但是执行到`sudo certbot certonly --nginx`，就报错找不到nginx的二进制文件

故在网上找到解决方案：[Let's Encrypt 使用教程，免费的SSL证书，让你的网站拥抱 HTTPS - Diamond-Blog (diamondfsd.com)](https://diamondfsd.com/lets-encrytp-hand-https/)

```
certbot certonly --webroot -w /var/www/example -d example.com -d www.example.com
certbot certonly --webroot -w /data/app/GameH5/client_web -d "wss.blazeze.com"
certbot certonly --webroot -w /data/app/GameH5/client_web -d "h5cdn.blazeze.com"
certbot certonly --webroot -w /data/app/GmWebSGame/dist -d "ligen9527.haoyue888.io"
```
	
	注意：
	这个命令会为 [example.com](http://example.com/) 和 [www.example.com](http://www.example.com/) 这两个域名生成一个证书，使用 `--webroot` 模式会在 `/var/www/example` 中创建 `.well-known` 文件夹，这个文件夹里面包含了一些验证文件，certbot 会通过访问 [example.com/.well-known/acme-challenge](http://example.com/.well-known/acme-challenge) 来验证你的域名是否绑定的这个服务器。这个命令在大多数情况下都可以满足需求，
	
	但是有些时候我们的一些服务并没有根目录，例如一些微服务，这时候使用 `--webroot` 就走不通了。certbot 还有另外一种模式 `--standalone` ， 这种模式不需要指定网站根目录，他会自动启用服务器的443端口，来验证域名的归属。我们有其他服务（例如nginx）占用了443端口，就必须先停止这些服务，在证书生成完毕后，再启用。


certbot certonly -d "*.smile13.com" -d "smile13.com" --manual --preferred-challenges dns-01
--server https://acme-v02.api.letsencrypt.org/directory

以上已经满足我将ssl证书应用到nginx中，至于用到微服务，以后需要再探索standalone模式