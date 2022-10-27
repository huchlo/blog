https://docs.gitlab.com/runner/install/

https://www.runoob.com/docker/windows-docker-install.html
Docker Pull命令

docker pull gitlab/gitlab-ce 社区版
docker pull gitlab/gitlab-ee 企业版
docker pull gitlab/gitlab-runner runner

centos7
https://mirror.tuna.tsinghua.edu.cn/help/gitlab-ce/
yum makecache && yum install gitlab-ce -y  （全部选y）
安装vim
yum install vim
配置文件在/etc/gitlab/gitlab.rb

安装gitlab-ce：gitlab-ctl reconfigure

运行：gitlab-ctl start

查看工具
yum install net-tools -y
查看命令
netstat -tlunp
gitlab-ctl status

注意防火墙设置：
firewall-cmd --add-service=http --permanent
systemctl reload firewalld

