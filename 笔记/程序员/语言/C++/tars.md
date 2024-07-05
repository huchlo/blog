替代品：
brpc 
trpc：[介绍 | tRPC](https://trpc.group/zh/docs/what-is-trpc/)

简介： https://doc.tarsyun.com/#/base/tars-intro.md

	不使用mysql，不使用framework，只用TarsCpp创建服务，linux环境下开发

编译环境 centos 7.9
>centos: yum install -y glibx-devel gcc gcc-c++ bison flex which psmisc ncurses-devel zlib-devel git wget 

>ubuntu: sudo apt-get install -y build-essential bison flex psmisc libncurses5-dev-zlib1g-dev get wget

下载编译cmake
环境:yum install openssl-devel
官网： https://cmake.org/download/
目前最新版本： http://github.com/Kitware/CMake/releases/download/v3.30.0/cmake-3.30.0.tar.gz
wget http://github.com/Kitware/CMake/releases/download/v3.30.0/cmake-3.30.0.tar.gz
tar xzf cmake.xxx.tar.gz
cd cmake.xxx
./configure
make
make install


```sh
git clone https://github.com/TarsCloud/TarsCpp.git --recursive
cd TarsCpp;
mkdir build;
cd build;
cmake ..
make -j4
make install
```

安装完后，会在/usr/local/tars/cpp、/home/tarsproto/protocol目录生成一些文件
在/usr/local/tars/cpp/script下有可用于创建初始服务的脚本
创建服务分两块：tars和http，cmake是用cmake编译，create用make编译
```bash
cmake_http_server.sh
cmake_tars_server.sh
create_http_server.sh
create_tars_server.sh
```

