---
description: 路印协议基础数据结构
---

# JSON Schema

## content:

1\) order structure

2\) trade structure

3\) ring structure

4\) how order hash \(id\) is calculated

5\) how order should be signed by the owner

6\) how a ring's hash is calculated

7\) how a ring is signed by all dual authoring keys

8\) how soft cancel order request is signed.

## Order  structure

```text
order: {
  owner: Address,
  version: HexString,
  tokenS: Address,
  tokenB: Address,
  amountS: HexString,
  amountB: HexString,
  validSince: HexString,  
  params: {
    broker: Address,
    dualAuthAddr: Address,
    dualAuthPrivateKey: HexString,
    validUntil: HexString,
    allOrNone：HexString,
    wallet: Address,
    orderInterceptor: Address,
    dualAuthSig: HexString,
    sig: HexString
  },
  feeParams: {
    tokenRecipient: Address,
    amountFee：HexString,
    tokenFee：Address,
    waiveFeePercentaget: HexString,
    tokenSFeePercentage: HexString,
    tokenBFeePercentage: HexString,
    walletSplitPercentage: HexString
  },
  ERC1400Params: {
    tokenStandardS: HexString,
    tokenStandardB: HexString,
    tokenStandardFee: HexString,
    trancheS: HexString,
    trancheS: HexString,
    transferDataS: HexString
  },
  signType: HexString
}
```

* validSince:  订单生效时间，seconds
* broker:  _订单代理地址（可选，当下单人与订单owner不同时，broker即为下单人地址。需要预先注册）_
*  _dualAuthAddr:_ 双重签名地址
* dualAuthPrivateKey:  _双重签名地址私钥,_
* _validUntil : seconds，可选，不设置时代表该订单永远有效_
* _allOrNone：_是否要求一次定完全成交，默认为false
* wallet : _前端钱包地址，参与手续费分成_
* _orderInterceptor:_  订单回调合约地址，暂时不支持，不需要设置
* dualAuthSig: _双重签名_
* _tokenRecipient: token 收款地址，一般为owner_
*  _waiveFeePercentage :_   miner设置，前端不需要设置，费率折扣设置
* tokenSFeePercentage :  _p2p 订单使用，tokenS收费比例_
* _tokenBFeePercentage:_  p2p 订单使用，tokenB收费比例 
* walletSplitPercentage: _钱包分成比例_
* _tokenStandardS : "0x0" 代表 ERC20 ，"0x1"代表ERC1400_
*  _tokenStandardB: "0x0" 代表 ERC20 ，"0x1"代表ERC1400_
*  _tokenStandardFee : "0x0" 代表 ERC20 ，"0x1"代表ERC1400_
*  _trancheS :_  erc1400 token 转账需要的参数
* trancheS: _erc1400 token 转账需要的参数_
* _transferDataS :_ erc1400 token 转账需要的参数 
*  signType: 0 代表Ethereum Sign ,1 代表EIP712签名方式

## Trade Structure

```text
trade: {
  owner: Address,
  orderHash: HexString,
  ringHash: HexString,
  ringIndex: HexString,
  fillIndex: HexString,
  txHash: HexString,
  amountS: HexString,
  amountB: HexString,
  tokenS: Address,
  tokenB: Address,
  marketKey: HexString,
  split: HexString,
  fee: {
    tokenFee: Address,
    amountFee: HexString,
    feeAmountS: HexString,
    feeAmountB: HexString,
    feeRecipient: Address,
    waiveFeePercentage: HexString,
    walletSplitPercentage: HexString
  },
  wallet: Address,
  miner: Address,
  blockHeight: HexString,
  blockTimestamp: HexString
}
```

* split : 交易时，矿工获得的价差
* tokenFee：当交易费的token地址
* amountFee：该撮合中该笔成交付的手续费
* feeAmountS: P2P交易中付出的tokens作为手续费，
* feeAmountB:P2P交易中付出的tokenB作为手续费
* feeRecipient: 接收手续费的地址
* waiveFeePercentage：矿工设置，免除手续费的比例
* walletSplitPercentage： DE设置，DEX要收取的交易费比例
* wallet：DEX收取交易费的地址
* miner：撮合订单的矿工地址

## Ring Structure

```text
ring: {
  ringHash: HexString,
  ringIndex: HexString,
  fillsAmount: HexString,
  miner: HexString,
  txHash: HexString,
  fees: [
    {
      tokenFee: Address,
      amountFee: HexString,
      feeAmountS: HexString,
      feeAmountB: HexString,
      feeRecipient: Address,
      waiveFeePercentage: HexString,
      walletSplitPercentage: HexString
    }
  ],
  blockHeight: HexString,
  blockTimestamp: HexString
}
```

* fillsAmount: the fills amount in this ring
* fees ：a list of Trade Fee

