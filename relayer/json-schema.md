---
description: 路印协议基础数据结构
---

# JSON Schema

## content:

1\) order statstructure

2\) how order hash \(id\) is calculated

3\) how order should be signed by the owner

4\) how a ring's hash is calculated

5\) how a ring is signed by all dual authoring keys

6\) how soft cancel order request is signed.



## Order订单结构

* owner : Address
* version : Hex String
* tokenS : Address
* tokenB : Address
* amountS : Hex String
* amountB : Hex String
* validSince : Hex String \(订单生效时间，seconds\)
* params:
  * broker: Address \* 订单代理地址（可选，当下单人与订单owner不同时，broker即为下单人地址。需要预先注册）
  * dualAuthAddr:Address \* 双重签名地址
  * dualAuthPrivateKey:Hex String \* 双重签名地址私钥
  * validUntil : Hex String \(seconds，可选，不设置时代表该订单永远有效\)
  * allOrNone：Hex String  \* 是否要求一次定完全成交，默认为false
  * wallet :Address \* 前端/钱包地址，参与手续费分成
  * orderInterceptor: Address \* 订单回调合约地址，暂时不支持，不需要设置
  * dualAuthSig: Hex String \* 双重签名
  * sig : Hex String
* feeParams:
  * tokenRecipient: Address（token 收款地址，一般为owner）
  * amountFee：Hex String
  * tokenFee：Address
  * waiveFeePercentaget : Hex String \* miner设置，前端不需要设置，费率折扣设置。
  * tokenSFeePercentage : Hex String  \* p2p 订单使用，tokenS收费比例
  * tokenBFeePercentage: Hex String \* p2p 订单使用，tokenB收费比例
  * walletSplitPercentage: Hex String  \* 钱包分成比例
* ERC1400Params:
  * tokenStandardS : Hex String \("0x0" 代表 ERC20 ，"0x1"代表ERC1400\)
  * tokenStandardB: Hex String \("0x0" 代表 ERC20 ，"0x1"代表ERC1400\)
  * tokenStandardFee : Hex String \("0x0" 代表 ERC20 ，"0x1"代表ERC1400\)
  * trancheS : Hex String \* erc1400 token 转账需要的参数
  * trancheS: Hex String \* erc1400 token 转账需要的参数
  * transferDataS : Hex String \* erc1400 token 转账需要的参数 
* signType: Hex String \(0 代表Ethereum Sign ,1 代表EIP712签名方式\)

