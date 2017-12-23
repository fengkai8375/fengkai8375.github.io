---
layout: post
title: "CentOS 6.8搭建L2TP VPN"
date: 2017-09-16 09:00:00 +0800 
categories: Linux
tag: VPN
---
* content
{:toc}

安装命令

```
cd /opt/ && wget --no-check-certificate https://raw.githubusercontent.com/teddysun/across/master/l2tp.sh
chmod +x l2tp.sh
./l2tp.sh install
```

<!-- more -->

安装过程中注意事项

1. IP-Range可以默认
2. PSK、用户名、密码最好自己指定

部署完成后需要修改

`vi  /etc/sysconfig/iptables`  
```
-A POSTROUTING -s 192.168.18.0/24 -j SNAT --to-source 172.31.103.51 
```
`to-source`的地址是服务器的内网地址

`vi /etc/xl2tpd/xl2tpd.conf`，修改`local ip` 为服务器内网ip
```
local ip = 172.31.103.51
```

最后重启 `iptables`和`xl2tpd`

```
service iptables restart
service xl2tpd restart
```

附上完整的iptables配置

```
# Added by L2TP VPN script
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -p tcp --dport 22 -j ACCEPT
-A INPUT -p udp -m multiport --dports 500,4500,1701 -j ACCEPT
-A FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT
-A FORWARD -s 192.168.18.0/24  -j ACCEPT
COMMIT
*nat
:PREROUTING ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
:POSTROUTING ACCEPT [0:0]
-A POSTROUTING -s 192.168.18.0/24 -j SNAT --to-source 172.31.103.51
COMMIT
```

xl2tpd配置

```
[global]
port = 1701

[lns default]
ip range = 192.168.18.2-192.168.18.254
local ip = 172.31.103.51
require chap = yes
refuse pap = yes
require authentication = yes
name = l2tpd
ppp debug = yes
pppoptfile = /etc/ppp/options.xl2tpd
length bit = yes
```