---
layout: post
title: "比特币钱包命令行交互"
date: 2018-11-11 09:00:00 +0800 
categories: BlockChain
tag:
 - Bitcoin
---
* content
{:toc}

以下是比特币钱包命令行交互的几个示例，会提示输入RPC密码。

#### getinfo
```
curl --user 'username' --data-binary '{"jsonrpc":"1.0","id":"1","method":"getinfo","params":[]}' -H 'content-type:text/plain;' http://127.0.0.1:1101
```

<!-- more -->

#### 加密钱包
```
curl --user 'username' --data-binary '{"jsonrpc":"1.0","id":"1","method":"encryptwallet","params":["pass_word"]}' -H 'content-type:text/plain;' http://127.0.0.1:1101
```


#### 解锁钱包

```
curl --user 'username' --data-binary '{"jsonrpc":"1.0","id":"1","method":"walletpassphrase","params":["pass_word",600]}' -H 'content-type:text/plain;' http://127.0.0.1:1101
```



