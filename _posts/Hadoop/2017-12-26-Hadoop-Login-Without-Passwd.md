---
layout: post
title: "Hadoop配置免密登录"
date: 2017-12-26 09:00:00 +0800 
categories: Hadoop
tag: Hadoop
---
* content
{:toc}

## 以下在Master主机上配置

输入以下指令生成ssh
```
ssh-keygen
// 会生成两个文件，放到默认的/root/.ssh/文件夹中

```

如果提示命令不存在，则需安装 ```openssh-server```和 ```openssh-client```

<!-- more -->

把id_rsa.pub追加到授权的key里面去
```
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

修改文件”authorized_keys”权限
```
chmod 600 ~/.ssh/authorized_keys
```

设置SSH配置
```
vi /etc/ssh/sshd_config
```

以下三项修改成以下配置
```
RSAAuthentication yes # 启用 RSA 认证

PubkeyAuthentication yes # 启用公钥私钥配对认证方式

AuthorizedKeysFile .ssh/authorized_keys # 公钥文件路径（和上面生成的文件同）
```

重启ssh服务
```
service sshd restart
```

把公钥复制所有的Slave机器上
```
// scp ~/.ssh/id_rsa.pub 远程用户名@远程服务器IP:~/

scp ~/.ssh/id_rsa.pub root@192.168.1.125:~/
scp ~/.ssh/id_rsa.pub root@192.168.1.124:~/
```

## 以下在Slave主机上配置

在slave主机上创建.ssh文件夹
```
mkdir ~/.ssh
```

修改权限
```
chmod 700 ~/.ssh
```

追加到授权文件”authorized_keys”
```
cat ~/id_rsa.pub >> ~/.ssh/authorized_keys
```

修改权限
```
chmod 600 ~/.ssh/authorized_keys
```

删除无用.pub文件
```
rm –r ~/id_rsa.pub
```


在master主机下进行测试
```
ssh 192.168.1.125
ssh 192.168.1.124
```
如果能够分别无密码登陆slave1, slave2主机，则成功配置

