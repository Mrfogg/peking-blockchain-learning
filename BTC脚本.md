## 脚本

比特币脚本语言，能访问的内存空间只有栈

### 交易实例

| 字段名        | 含义                               |
| ------------- | ---------------------------------- |
| Txid          | 交易id                             |
| Hash          | 交易哈希值                         |
| version       | 协议版本号                         |
| size          | 交易大小                           |
| Locktime      | 用来设定交易生效时间,0代表立即生效 |
| Vin           |                                    |
| Vout          |                                    |
| Confirmations |                                    |
| Blockhash     | 区块哈希值，挖矿算出来的           |
| time          | 交易产生时间                       |
| Blocktime     | 区块产生时间                       |

**vin**

交易的输入结构

```json
{
  "vin" :[{
    "txid" :"c0cb...c57v", //之前交易的哈希值
    "vout":0, //之前交易的第几个输出
    "scriptSig":{   //输入脚本
      "asm":"3045...0018",
      "hex":"4830...0018"
    }
  }]
}
```

**vout**

交易的输出结构

```json
{
  "vout":[{
    "value":0.2268400, //金额
    "n":0, // 这个交易的第几个输出
    "scriptPubKey":{
      "asm":"DUP HASH160 628e...d743 EQUALVERIFY CHECKSIG", //输出脚本内容
      "hex":"76a9...99ac",
      "reqSigs":1, //需要多少个签名
      "type":"pubkeyhash", //输出类型
      "addresses":["fljdlajflkjdaeunaoqh"] //输出地址
    }
  }]
}
```

### 脚本执行

**验证合法性**

A->B 和B->C的脚本，拼接在一起执行

1. PUSHDATA(Sig)  输入脚本签名入栈
2. PUSHDATA(PubKey) 输出脚本公钥压栈
3. CHECKSIG  两个元素弹出来，用公钥检测签名是否正确

**P2PKH**

输出脚本里没有直接给出收款人的公钥，给出的是公钥的哈希

输入脚本

1. PUSHDATA(sig)  签名入栈
2. PUSHDATA(PubKey)  公钥入栈

输出脚本

1. DUP 栈顶元素复制
2. HASH160  把栈顶元素弹出取哈希再入栈
3. PUSHDATA(PubKeyHash) 输出脚本公钥的哈希值入栈
4. EQUALVERIFY 比如栈顶两个哈希值是否相等
5. CHECKSIG  弹出两个元素，用公钥检测签名是否正确

中间过程任何错误，则这个交易就是非法的

**P2SH(Pay to Script Hash)**

为什么要这么复杂，对多重签名的支持





