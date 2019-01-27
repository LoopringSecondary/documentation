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

{% api-method method="post" %}
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

{% api-method-parameter name="params" type="object" required=false %}

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
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="" type="string" required=false %}

{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "name": "Cake's name",
    "recipe": "Cake's recipe name",
    "cake": "Binary cake"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

