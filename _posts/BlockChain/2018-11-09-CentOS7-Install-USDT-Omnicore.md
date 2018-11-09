---
layout: post
title: "CentOS 7配置Omnicore USDT钱包"
date: 2018-11-09 09:00:00 +0800 
categories: BlockChain
tag:
 - CentOS
---
* content
{:toc}

本文介绍了如何在CentOS 7下配置Omnicore USDT钱包。

#### 安装依赖环境
```
yum -y install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils

yum -y install gcc-c++ libtool make autoconf automake openssl-devel libevent-devel boost-devel libdb4-devel libdb4-cxx-devel

yum -y install qt5-qttools-devel qt5-qtbase-devel protobuf-devel 
```

<!-- more -->

#### 编译安装db4.8
```
wget http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz

tar -xzvf db-4.8.30.NC.tar.gz  

cd db-4.8.30.NC/build_unix 

../dist/configure --enable-cxx --disable-shared --with-pic --prefix=$BDB_PREFIX
make 
make install
```

#### 下载omnicore源码
```
git clone https://github.com/OmniLayer/omnicore.git
```

#### 编译安装omnicore

```
cd omnicore/

./autogen.sh
./configure LDFLAGS="-L/home/db4/lib/" CPPFLAGS="-I/home/db4/include/"      //此处要指定3步编译完的db位置

make && make install
```

