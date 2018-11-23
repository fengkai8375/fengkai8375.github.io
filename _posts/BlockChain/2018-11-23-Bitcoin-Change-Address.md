---
layout: post
title: "Bitcoin交易中的找零地址Change Address"
date: 2018-11-23 09:00:00 +0800 
categories: BlockChain
tag:
 - Bitcoin
---
* content
{:toc}

`Change Address`找零地址是Bitcoin交易中地址余额大于实际发送额时产生的地址。

比如地址A余额10 BTC，向一个地址B发送1 BTC，此时会自动生成一个地址C，接收扣除手续后的余额，这个地址C就是Change Address。

即input是地址A，output是接收地址B和找零地址C。

避免产生找零地址似乎只有一个方法：一次性转完，但实际情况下往往不是这样。

产生的找零的私钥会存储在钱包中，所以安全起见，BTC官方建议每产生50笔新交易就备份一下`wallet.dat`。

