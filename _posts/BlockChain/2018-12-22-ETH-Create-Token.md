---
layout: post
title: "ETH 生成自己的Token"
date: 2018-12-22 09:00:00 +0800 
categories: BlockChain
tag:
 - ETH
---
* content
{:toc}

### 工具

1. Chrome扩展metamask
2. 合约编译工具 [remix](http://remix.ethereum.org)


示例合约
> 注意保存好自己的合约源代码，留待验证时使用！！！

<!-- more -->

```
// Abstract contract for the full ERC 20 Token standard
pragma solidity ^0.4.12;

contract Token {
    
    uint256 public totalSupply;

    function balanceOf(address _owner) constant returns (uint256 balance);

    function transfer(address _to, uint256 _value) returns (bool success);

    function transferFrom(address _from, address _to, uint256 _value) returns (bool success);

    function approve(address _spender, uint256 _value) returns (bool success);

    function allowance(address _owner, address _spender) constant returns (uint256 remaining);

    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);
}

/*
You should inherit from StandardToken or, for a token like you would want to
deploy in something like Mist, see WalStandardToken.sol.
(This implements ONLY the standard functions and NOTHING else.
If you deploy this, you won't have anything useful.)

Implements ERC 20 Token standard: https://github.com/ethereum/EIPs/issues/20
.*/

contract StandardToken is Token {

    function transfer(address _to, uint256 _value) returns (bool success) {
        //Default assumes totalSupply can't be over max (2^256 - 1).
        //If your token leaves out totalSupply and can issue more tokens as time goes on, you need to check if it doesn't wrap.
        //Replace the if with this one instead.
        //if (balances[msg.sender] >= _value && balances[_to] + _value > balances[_to]) {
        if (balances[msg.sender] >= _value && _value > 0) {
            balances[msg.sender] -= _value;
            balances[_to] += _value;
            Transfer(msg.sender, _to, _value);
            return true;
        } else { return false; }
    }

    function transferFrom(address _from, address _to, uint256 _value) returns (bool success) {
        //same as above. Replace this line with the following if you want to protect against wrapping uints.
        //if (balances[_from] >= _value && allowed[_from][msg.sender] >= _value && balances[_to] + _value > balances[_to]) {
        if (balances[_from] >= _value && allowed[_from][msg.sender] >= _value && _value > 0) {
            balances[_to] += _value;
            balances[_from] -= _value;
            allowed[_from][msg.sender] -= _value;
            Transfer(_from, _to, _value);
            return true;
        } else { return false; }
    }

    function balanceOf(address _owner) constant returns (uint256 balance) {
        return balances[_owner];
    }

    function approve(address _spender, uint256 _value) returns (bool success) {
        allowed[msg.sender][_spender] = _value;
        Approval(msg.sender, _spender, _value);
        return true;
    }

    function allowance(address _owner, address _spender) constant returns (uint256 remaining) {
      return allowed[_owner][_spender];
    }

    mapping (address => uint256) balances;
    mapping (address => mapping (address => uint256)) allowed;
}

contract MyToken is StandardToken {

    /* Public variables of the token */
    /*
    NOTE:
    The following variables are OPTIONAL vanities. One does not have to include them.
    They allow one to customise the token contract & in no way influences the core functionality.
    Some wallets/interfaces might not even bother to look at this information.
    */
    string public name;                   //fancy name: eg Simon Bucks
    uint8 public decimals;                //How many decimals to show. ie. There could 1000 base units with 3 decimals. Meaning 0.980 SBX = 980 base units. It's like comparing 1 wei to 1 ether.
    string public symbol;                 //An identifier: eg SBX
    string public version = "1.0";       // 0.1 standard. Just an arbitrary versioning scheme.
    function MyTokenInit() {
        balances[msg.sender] = 10000000000000000;               // Give the creator all initial tokens
        totalSupply = 10000000000000000;                        // Update total supply
        name = "MyToken";                                   // Set the name for display purposes
        decimals = 8;                            // Amount of decimals for display purposes
        symbol = "MyToken";                               // Set the symbol for display purposes
    }
}
```

修改的内容是最后一个类， 合约名称、初始量、给合约生成者的量等，注意初始量和给合约生成者的量是实际总量乘以10的精度次方。

### 步骤

1. 安装Chrome的metamask扩展
2. 生成账号并充值ETH，整个流程下来大约需要0.02个ETH
3. 打开remix，新建文件，复制以上合约到编辑框中并做相应的修改
4. 选择合适的编译器版本，这里用的是0.4.12，点击`compile`
5. 编译完成后切换到`Run`选项卡，点击`Deploy`，并在metamask弹出的窗口中确认
6. 合约生成完成后，调用`MyTokenInit`初始化
7. 调用`transfer`方法，将初始化好的Token转移给自己或另一个地址。
8. 结束。

