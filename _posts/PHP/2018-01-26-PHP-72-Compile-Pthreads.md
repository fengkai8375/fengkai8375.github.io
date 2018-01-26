---
layout: post
title: "PHP7.2编译安装及pthreads多线程配置"
date: 2018-01-26 09:00:00 +0800 
categories: PHP
tag:
 - PHP
 - pthreads
---
* content
{:toc}

### 编译安装PHP7.2

```shell
wget http://cn2.php.net/distributions/php-7.2.1.tar.gz
tar -zxvf php-7.2.1.tar.gz
cd php-7.2.1/

//安装依赖库
yum install gcc libxml2 libxml2-devel libcurl libcurl-devel openssl-devel gdbm-devel
yum install libwebp* libpng* libXpm* libjpeg* freetype*

//configure
./configure --prefix=/usr/local/php7 --exec-prefix=/usr/local/php7 --bindir=/usr/local/php7/bin --sbindir=/usr/local/php7/sbin --includedir=/usr/local/php7/include --libdir=/usr/local/php7/lib/php --mandir=/usr/local/php7/php/man --with-config-file-path=/usr/local/php7/etc --with-mysql-sock=/var/lib/mysql/mysql.sock --with-mhash --with-mysqli=shared,mysqlnd --with-pdo-mysql=shared,mysqlnd --with-gd --with-iconv --with-zlib --enable-zip --enable-inline-optimization --disable-debug --disable-rpath --enable-shared --enable-xml --enable-bcmath --enable-shmop --enable-sysvsem --enable-mbregex --enable-mbstring --enable-ftp --enable-pcntl --enable-sockets --with-xmlrpc --enable-soap --with-pear --with-gettext --enable-session --with-curl --with-openssl --with-jpeg-dir --with-freetype-dir --enable-opcache --enable-fpm --with-fpm-user=nginx --with-fpm-group=nginx --with-gdbm --enable-fileinfo --enable-maintainer-zts

//编译安装
make
make install

//拷贝php配置文件
cp php.ini-production /usr/local/php7/etc/php.ini
cp php.ini-production /usr/local/php7/etc/php-cli.ini
```

<!-- more -->

> 注意：pthreads不可在php-fpm中使用，只能在cli使用。

### 配置PHP环境变量

`vi /etc/profile`，内容如下：

```shell
PATH=$PATH:/usr/local/php7/bin
export PATH
```

`source /etc/profile`使之生效。


### 安装pthreads扩展
```shell
git clone https://github.com/krakjoe/pthreads.git
cd pthreads
phpize
./configure
make
make install

extension=pthreads //php-cli.ini
```

### 配置php-fpm启动停止脚本

`/etc/init.d/php-fpm`，内容如下：

```shell
##---start ---
#!/bin/bash
# php-fpm startup script for the php-fpm
# php-fpm version:7.2
# processname: php-fpm
# pidfile: /var/run/php-fpm.pid
# config: /usr/local/php7/etc/php-fpm.conf

BINFILE="/usr/local/php7/sbin/php-fpm"
CFGFILE="/usr/local/php7/etc/php-fpm.conf"
PIDFILE="/usr/local/php7/var/run/php-fpm.pid"
LOCKFILE="/var/lock/php-fpm.lock"

RETVAL=0
prog="php-fpm"


#start function
php_fpm_start() {
    /usr/local/php7/sbin/php-fpm -c /usr/local/php7/etc/php.ini -y /usr/local/php7/etc/php-fpm.conf
}


start() {
    [[ -x $BINFILE ]] || exit 5
    [[ -f $CFGFILE ]] || exit 6

    if [[ `ps aux | grep php-fpm: | grep -v grep | wc -l` -gt 0 ]]; then
        echo "The php-fpm is already running."
        return 1
    fi

    $BINFILE -t >/dev/null 2>&1

    if [[ $? -ne 0 ]]; then
        echo "The php-fpm configure has error."
        return 1
    fi

    echo -n "Starting php-fpm......"
    $BINFILE -y $CFGFILE -g ${PIDFILE}
    RETVAL=$?
    echo
    [[ $RETVAL -eq 0 ]] && touch $LOCKFILE

    return $RETVAL
}

stop() {
    if [[ `ps aux | grep php-fpm: | grep -v grep | wc -l` -eq 0 ]]; then
        echo "The php-fpm is not running."
        return 1
    fi

    echo -n "Shutting down php-fpm......"

    if [[ -f $PIDFILE ]]; then
        kill -QUIT `cat ${PIDFILE}`
    else
        kill -QUIT `ps aux | grep php-fpm | awk '/master/{print $2}'`
    fi

    RETVAL=$?
    echo
    [[ $RETVAL -eq 0 ]] && rm -f $LOCKFILE $PIDFILE

    return $RETVAL
}

restart() {
    stop
    sleep 1

    while true
    do
        if [[ `ps aux | grep php-fpm: | grep -v grep | wc -l` -eq 0 ]]; then
            start
            break
        fi
        sleep 1
    done

    RETVAL=$?
    echo

    return $RETVAL
}

reload() {
    if [[ `ps aux | grep php-fpm: | grep -v grep | wc -l` -eq 0 ]]; then
        echo "The php-fpm is not running."
        return 1
    fi

    echo -n $"Reloading php-fpm......"

    if [[ -f $PIDFILE ]]; then
        kill -USR2 `cat ${PIDFILE}`
    else
        kill -USR2 `ps aux | grep php-fpm | awk '/master/{print $2}'`
    fi

    RETVAL=$?
    echo

    return $RETVAL
}


# See how we were called.
case "$1" in
start)
        start
        ;;
stop)
        stop
        ;;
reload)
	reload
	;;
restart)
        restart
        ;;
status)
        status $prog
        RETVAL=$?
        ;;
*)
        echo $"Usage: $prog {start|stop|restart|status}"
        exit 1
esac
exit $RETVAL


##----end -----

```

