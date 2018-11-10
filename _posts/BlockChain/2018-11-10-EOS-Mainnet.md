---
layout: post
title: "EOS主网对接"
date: 2018-11-10 09:00:00 +0800 
categories: BlockChain
tag:
 - EOS
---
* content
{:toc}

该教程主要关于EOS钱包安装、配置及主网对接。
 
### 主要组件

1. nodeos
2. keosd
3. cleos

### 主要概念

1. wallet
2. 私钥/公钥
3. owner/active
4. 账户/子账户
5. RAM/NET/CPU

<!-- more -->

## 安装EOS

### 编译安装
```
//从github克隆主网代码仓库

git clone https://github.com/EOS-Mainnet/eos

cd eos

//更新代码仓库子模块，使用递归参数
git submodule update --init --recursive


//git tag命令查看版本标签
git tag

//本地仓库代码切换到mainnet-1.4.1
git checkout mainnet-1.4.1


//查看本地仓库是否是mainnet-1.4.1
git branch

//编译，注意机器内存至少需要7G
./eosio_build.sh


//编辑完成后安装
./eosio_install.sh
```

### 配置文件

Linux下的配置文件位置 `~/.local/share/eosio/nodeos/config/`

1. config.ini ，配置peer节点和plugins

```
chain-state-db-size-mb = 102400 //这个得设置大一点，默认1024

plugin = eosio::chain_api_plugin
 
plugin = eosio::history_api_plugin
 
plugin = eosio::chain_plugin
 
plugin = eosio::history_plugin
 
plugin = eosio::net_plugin
 
plugin = eosio::net_api_plugin
```

2. genesis.json 

```

{
  "initial_timestamp": "2018-06-08T08:08:08.888",
  "initial_key": "EOS7EarnUhcyYqmdnPon8rm7mBCTnBoot6o7fE2WzjvEX2TdggbL3",
  "initial_configuration": {
    "max_block_net_usage": 1048576,
    "target_block_net_usage_pct": 1000,
    "max_transaction_net_usage": 524288,
    "base_per_transaction_net_usage": 12,
    "net_usage_leeway": 500,
    "context_free_discount_net_usage_num": 20,
    "context_free_discount_net_usage_den": 100,
    "max_block_cpu_usage": 200000,
    "target_block_cpu_usage_pct": 1000,
    "max_transaction_cpu_usage": 150000,
    "min_transaction_cpu_usage": 100,
    "max_transaction_lifetime": 3600,
    "deferred_trx_expiration_window": 600,
    "max_transaction_delay": 3888000,
    "max_inline_action_size": 4096,
    "max_inline_action_depth": 4,
    "max_authority_depth": 6
  }
}
```

### 启动进程

```
//第一次启动需要指定genesis.json文件
./nodeos --genesis-json genesis.json

//以后启动
nodeos > /dev/null 2>&1 &
```


启动看通过`cleos get info`查看`chain_id`是否是
```
aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906
```

以确保节点是在主网上同步数据。

> Linux下的节点数据目录 `~/.local/share/eosio/nodeos/data/`，`genesis.json`文件变动时，需要清空该目录下的文件。

## 钱包交互


### 创建钱包
```
# cleos wallet create --to-console

Creating wallet: default
Save password to use in the future to unlock this wallet.
Without password imported keys will not be retrievable.
"PW5KQSBWzJCt1kSZnH1UW6cLiU5HpftmkRiXcpxi7A5dGECFyN7nq"
```

### 解锁

```
# cleos wallet unlock --password PW5KQSBWzJCt1kSZnH1UW6cLiU5HpftmkRiXcpxi7A5dGECFyN7nq
```

### 加锁

```
# cleos wallet lock
```

### 创建owner密钥对
```
# cleos create key --to-console
Private key: 5K9RowyzRHnhAFcbe3PPaoy1KMKcXadybEFoNC4Gpbga4ak6H1Y
Public key: EOS7nyAheoTBLSVj7NL24dU9r4rPgdTT1aPiRNexeueiBsMXoapro
```

### 创建active密钥对

```
# cleos create key --to-console
Private key: 5KHjf1QKTAGZ3nD5E5rSNw65wx33AqgV8BzUK7RwJ6zdNAmSjp6
Public key: EOS8NTgeris3xhTS6dtSgn4FhCu1NqeXvdk1PzEKNjFbEx7viK4CX
```

### 导入owner和active密钥到钱包
```
# cleos wallet import --private-key 5K9RowyzRHnhAFcbe3PPaoy1KMKcXadybEFoNC4Gpbga4ak6H1Y
imported private key for: EOS7nyAheoTBLSVj7NL24dU9r4rPgdTT1aPiRNexeueiBsMXoapro

#cleos wallet import --private-key 5KHjf1QKTAGZ3nD5E5rSNw65wx33AqgV8BzUK7RwJ6zdNAmSjp6
imported private key for: EOS8NTgeris3xhTS6dtSgn4FhCu1NqeXvdk1PzEKNjFbEx7viK4CX
```

### 创建账号

```
# cleos create account eosio cdcgogo EOS7nyAheoTBLSVj7NL24dU9r4rPgdTT1aPiRNexeueiBsMXoapro EOS8NTgeris3xhTS6dtSgn4FhCu1NqeXvdk1PzEKNjFbEx7viK4CX

Error 3090003: Provided keys, permissions, and delays do not satisfy declared authorizations
Ensure that you have the related private keys inside your wallet and your wallet is unlocked.
```

貌似需要节点完全同步后才可以创建，因为创建账号是同步到全网络的。


创建账户时购买RAM，同时抵押CPU和NET
```
cleos system newaccount --stake-net '1 EOS' --stake-cpu '1 EOS' --buy-ram-kbytes 20 eosio cdcgogo EOS7nyAheoTBLSVj7NL24dU9r4rPgdTT1aPiRNexeueiBsMXoapro EOS8NTgeris3xhTS6dtSgn4FhCu1NqeXvdk1PzEKNjFbEx7viK4CX
```

### 购买RAM
```
cleos system buyram cdcgogo cdcgogo "10 EOS"
```

### 抵押NET和CPU
```
cleos system delegatebw cdcgogo cdcgogo "10 EOS" "10 EOS" 
```
最后两个参数是net和cpu要抵押的量，其中一个可为0

### 获取账号信息

```
# cleos get account cdcgogo
```

### 转账

```
cleos transfer from_account to_account "10 EOS" "memo"
```

### 启动Keosd

```
keosd --config-dir "/root/.local/share/eosio/keosd"
//keosd目录是自己创建的，方便起见和nodeos目录放在一起
```

keosd配置 `config.ini`，放到上文的目录下。

```
http-server-address= 0.0.0.0:8889
access-control-allow-origin = "*" 
wallet-dir = "/root/eosio-wallet"
access-control-allow-credentials =  false
http-validate-host = 0
unix-socket-path = keosd.sock
```

其实就是把 1.3版本之前 nodeos config.ini中的一部分拿过来. 1.3版本之后，nodeos和keosd独立开来， nodeos config.ini不再包含 `wallet-dir`和`unlock-timeout`两项，所以nodeos的api也不再包含`/v1/wallet`，需要为keosd另外配置一个http端口。

### 参考

[Github](https://github.com/EOS-Mainnet/eos)

[源码安装及测试](https://blog.csdn.net/caokun_8341/article/details/80656765)

[RPC API](https://developers.eos.io/eosio-nodeos/reference)

[Wallet API](https://developers.eos.io/keosd/reference)

[节点列表](https://eosnodes.privex.io) 

[EOS CLI创建钱包和账号](https://www.jianshu.com/p/e7c3a3b4044f)

[EOS RPC创建账号](https://www.jianshu.com/p/f2b4154a4022)

[EOS 开发系列教程](https://www.jianshu.com/u/e372dabaafaf)

[区块浏览器](https://eospark.com)

[Public Api](https://www.eosdocs.io/resources/apiendpoints/)