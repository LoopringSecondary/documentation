---
description: 光锥中继API中文文档
---

# Standard API

### Order订单结构

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

### RPC 接口

#### 基础接口

{% api-method method="post" path="" %}
{% api-method-summary %}
getServerTime
{% endapi-method-summary %}

{% api-method-description %}
获取server的时间，单位是毫秒。
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="jsonrpc" type="string" required=true %}
"2.0"
{% endapi-method-parameter %}

{% api-method-parameter name="method" type="string" required=true %}
"loopring\_getServerTime"
{% endapi-method-parameter %}

{% api-method-parameter name="params" type="array" required=true %}
\[\]
{% endapi-method-parameter %}

{% api-method-parameter name="id" type="string" required=true %}
string 或者 integer
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "id":1,
  "jsonrpc": "2.0",
  "result": 1548410119809
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "id": 1,
  "jsonrpc": "2.0",
  "error": {
    "code": -32603,
    "message": ""
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" %}
{% api-method-summary %}
 getTokenMetaData
{% endapi-method-summary %}

{% api-method-description %}
 获取Relay支持的token的信息，包括最小下单量，最大下单量，保留小数位，地址, symbol,decimal等
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="jsonrpc" type="string" required=true %}
"2.0"
{% endapi-method-parameter %}

{% api-method-parameter name="method" type="string" required=true %}
"loopring\_getTokenMetaData"
{% endapi-method-parameter %}

{% api-method-parameter name="params" type="array" required=true %}
 返回指定token的信息，若不指定，应该返回全部支持的token的信息。如: \["LRC","WETH"\]
{% endapi-method-parameter %}

{% api-method-parameter name="id" type="string" required=true %}
 可以为string或integer
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": [
    {
      "minTradeAmount": 0,
      "maxTradeAmount": 100000000,
      "precision": 5,
      "decimal": 18,
      "symbol": "LRC",
      "address": "0xef68e7c694f40c8202821edf525de3782458639f"
    },
    {
      "minTradeAmount": 0,
      "maxTradeAmount": 100000000,
      "precision": 5,
      "decimal": 18,
      "symbol": "WETH",
      "address": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
    }
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "id": 1,
  "jsonrpc": "2.0",
  "error": {
    "code": -32603,
    "message": ""
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" %}
{% api-method-summary %}
getMarketMetaData
{% endapi-method-summary %}

{% api-method-description %}
获取Relay支持的市场信息。
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="jsonrpc" type="string" required=true %}
"2.0"
{% endapi-method-parameter %}

{% api-method-parameter name="method" type="string" required=true %}
"loopring\_getMarketMetaData"
{% endapi-method-parameter %}

{% api-method-parameter name="params" type="array" required=true %}
 返回指定市场的信息，若不指定，则应该返回全部支持的市场信息。如：\["LRC-WETH"\]
{% endapi-method-parameter %}

{% api-method-parameter name="id" type="string" required=true %}
 string或者integer
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": [
    {
      "symbol": "LRC-WETH",
      "precisionForAmount": 5,
      "precisionForTotal": 8,
      "precisionForPrice": 5
    }
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "id": 1,
  "jsonrpc": "2.0",
  "error": {
    "code": -32603,
    "message": ""
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" %}
{% api-method-summary %}
 getAssets
{% endapi-method-summary %}

{% api-method-description %}
 获取指定owner 以及token的余额和授权，如果不指定，则应该返回全部支持的token的余额和授权。
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="jsonrpc" type="string" required=true %}
2.0
{% endapi-method-parameter %}

{% api-method-parameter name="method" type="string" required=true %}
 loopring\_getAssets
{% endapi-method-parameter %}

{% api-method-parameter name="params" type="array" required=true %}
 如：\["LRC","WETH"\]
{% endapi-method-parameter %}

{% api-method-parameter name="id" type="string" required=true %}

{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
    "records": [
      {
        "symbol": "LRC",
        "balance": "0x1326beb03e0a0000",
        "allowance": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
      },
      ...
    ]
  }
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "id": 1,
  "jsonrpc": "2.0",
  "error": {
    "code": -32603,
    "message": ""
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="" %}
{% api-method-summary %}
 submitOrder
{% endapi-method-summary %}

{% api-method-description %}
 用户提交订单
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="jsonrpc" type="string" required=false %}
2.0
{% endapi-method-parameter %}

{% api-method-parameter name="method" type="string" required=false %}
loopring\_submitOrder
{% endapi-method-parameter %}

{% api-method-parameter name="params" type="array" required=false %}

{% endapi-method-parameter %}

{% api-method-parameter name="id" type="string" required=false %}

{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "id":1,
  "jsonrpc": "2.0",
  "result":"0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=304 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "id": 1,
  "jsonrpc": "2.0",
  "error": {
    "code": -32603,
    "message": "invalid sig"
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" %}
{% api-method-summary %}
 cancelOrder
{% endapi-method-summary %}

{% api-method-description %}
 用户取消指定OrderHash的订单
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="jsonrpc" type="string" required=true %}
 2.0
{% endapi-method-parameter %}

{% api-method-parameter name="method" type="string" required=true %}
  loopring\_cancelOrder
{% endapi-method-parameter %}

{% api-method-parameter name="params" type="array" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="id" type="string" required=true %}

{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "id":1,
  "jsonrpc": "2.0",
  "result":"0xfff19b90ccf6eb051dd71aa29350acbcaedbbfb841e66c202fda5bf7bd084b85"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=304 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "id": 1,
  "jsonrpc": "2.0",
  "error": {
    "code": -32603,
    "message": "invalid sig"
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" %}
{% api-method-summary %}
 cancelOrders
{% endapi-method-summary %}

{% api-method-description %}
 取消指定市场对的全部订单，如果没有指定市场，则取消全部订单。
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="jsonrpc" type="string" required=true %}
2.0
{% endapi-method-parameter %}

{% api-method-parameter name="method" type="string" required=true %}
  loopring\_cancelOrders
{% endapi-method-parameter %}

{% api-method-parameter name="params" type="array" required=true %}
\["LRC-WETH"\]
{% endapi-method-parameter %}

{% api-method-parameter name="id" type="string" required=true %}
loopring\_cancelOrder
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "jsonrpc": "2.0",
  "method": "loopring_cancelOrders",
  "params": [
    {
      "market": "LRC-WETH"
    }
  ],
  "id": 1
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=304 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "id": 1,
  "jsonrpc": "2.0",
  "error": {
    "code": -32603,
    "message": "invalid sig"
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" %}
{% api-method-summary %}
 getOrders
{% endapi-method-summary %}

{% api-method-description %}
获取用户的订单列表  
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="jsonrpc" type="string" required=true %}
2.0
{% endapi-method-parameter %}

{% api-method-parameter name="method " type="string" required=true %}
loopring\_getOrders
{% endapi-method-parameter %}

{% api-method-parameter name="params" type="array" required=true %}
 
{% endapi-method-parameter %}

{% api-method-parameter name="id" type="string" required=true %}

{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "pageNum": 1,
    "pageSize": 50,
    "total": 60,
    "records": [
      {
        {
          "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
          "version": "0x0",
          "tokenS": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
          "tokenB": "0xef68e7c694f40c8202821edf525de3782458639f",
          "amountS": "0xde0b6b3a7640000",
          "validSince": "0x5c4b0cb3",
          "amountB": "0x3635c9adc5dea00000",
          "params": {
            "validUnit": "0x5c4cacb3",
            "allOrNone": "0x0",
            "sig": "0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b",
            "dualAuthAddr": "0x7ebdf3751f63a5fc1742ba98ee34392ce82fa8dd",
            "dualAuthPrivateKey": "0xc3d695ee4fcb7f14b8cf08a1d588736264ff0d34d6b9b0893a820fe01d1086a6"
          },
          "feeParams": {
            "tokenFee": "0xef68e7c694f40c8202821edf525de3782458639f",
            "amountFee": "0xde0b6b3a7640000"
          },
          "signType": "0x0"
        }
      },
      ...
    ]
  }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "id": 1,
  "jsonrpc": "2.0",
  "error": {
    "code": -32603,
    "message": ""
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" %}
{% api-method-summary %}
 getOrderByHash
{% endapi-method-summary %}

{% api-method-description %}
 根据OrderHash查询单条Order记录
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="jsonrpc" type="string" required=true %}
2.0
{% endapi-method-parameter %}

{% api-method-parameter name="method" type="string" required=true %}
loopring\_getOrderByHash
{% endapi-method-parameter %}

{% api-method-parameter name="params" type="array" required=true %}
\["0xfff19b90ccf6eb051dd71aa29350acbcaedb  
bfb841e66c202fda5bf7bd084b85"\]
{% endapi-method-parameter %}

{% api-method-parameter name="id" type="string" required=true %}

{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    {
      "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
      "version": "0x0",
      "tokenS": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
      "tokenB": "0xef68e7c694f40c8202821edf525de3782458639f",
      "amountS": "0xde0b6b3a7640000",
      "validSince": "0x5c4b0cb3",
      "amountB": "0x3635c9adc5dea00000",
      "params": {
        "validUnit": "0x5c4cacb3",
        "allOrNone": "0x0",
        "sig": "0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b",
        "dualAuthAddr": "0x7ebdf3751f63a5fc1742ba98ee34392ce82fa8dd",
        "dualAuthPrivateKey": "0xc3d695ee4fcb7f14b8cf08a1d588736264ff0d34d6b9b0893a820fe01d1086a6"
      },
      "feeParams": {
        "tokenFee": "0xef68e7c694f40c8202821edf525de3782458639f",
        "amountFee": "0xde0b6b3a7640000"
      },
      "signType": "0x0"
    }
  }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "id": 1,
  "jsonrpc": "2.0",
  "error": {
    "code": -32603,
    "message": ""
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

reque

{% api-method method="post" host="" path="" %}
{% api-method-summary %}
 getTrades
{% endapi-method-summary %}

{% api-method-description %}
获取用户的成交记录  
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="jsonrpc" type="string" required=false %}
2.0
{% endapi-method-parameter %}

{% api-method-parameter name="method" type="string" required=false %}
 loopring\_getTrades
{% endapi-method-parameter %}

{% api-method-parameter name="params" type="string" required=false %}

{% endapi-method-parameter %}

{% api-method-parameter name="id" type="string" required=true %}

{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "pageNum": 1,
    "pageSize": 50,
    "total": 60,
    "records": [
      {
        "orderHash": "",
        "ringHash": "",
        "tokenS": "0xef68e7c694f40c8202821edf525de3782458639f",
        "tokenB": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
        "tokenFee": "0xef68e7c694f40c8202821edf525de3782458639f",
        "amountS": "0xa",
        "amountB": "0x1",
        "amountFee": "0x2",
        "time": "0x5c4add07",
        "txHash": "0x9ab523ac966a375f02c5b22e275a6e4c9c621f83881650587bc331e95ee5e73"
      },
      ...
    ]
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% tabs %}
{% tab title="First Tab" %}
```text
{
  "jsonrpc": "2.0",
  "method": "loopring_getTrades",
  "params": [
    {
      "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
      "market": "LRC-WETH",
      "pageNum": 1,
      "pageSize": 50
    }
  ],
  "id": 1
}
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

