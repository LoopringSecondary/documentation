---
description: 路印协议基础数据结构
---

# JSON Schema

## Order订单结构

* owner : Address
* version : Hex String
* tokenS : Address
* tokenB : Address
* amountS : Hex String
* amountB : Hex String
* validSince : Hex String \(seconds\)
* params:
  * broker: Address （可选）
  * dualAuthAddr:Address
  * dualAuthPrivateKey:Hex String
  * validUntil : Hex String \(seconds，可选，不设置时代表该订单永远有效\)
  * allOrNone：Hex String
  * wallet :Address \* 不确定含义
  * orderInterceptor: Address \* 不确定含义
  * dualAuthSig: Hex String
  * sig : Hex String
* feeParams:
  * tokenRecipient: Address（token 收款地址，一般为owner）
  * amountFee：Hex String
  * tokenFee：Address
  * waiveFeePercentaget : Hex String
  * tokenSFeePercentage : Hex String
  * tokenBFeePercentage: Hex String
  * walletSplitPercentage: Hex String
* ERC1400Params:
  * tokenStandardS : Hex String \("0x0" 代表 ERC20 ，"0x1"代表ERC1400\)
  * tokenStandardB: Hex String \("0x0" 代表 ERC20 ，"0x1"代表ERC1400\)
  * tokenStandardFee : Hex String \("0x0" 代表 ERC20 ，"0x1"代表ERC1400\)
  * trancheS : Hex String \* 不确定含义
  * trancheS: Hex String \* 不确定含义
  * transferDataS : Hex String \* 不确定含义
* signType: Hex String \(0 代表Ethereum Sign ,1 代表EIP712签名方式\)

