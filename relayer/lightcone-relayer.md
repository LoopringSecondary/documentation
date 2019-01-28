---
description: 光锥中介中文API 文档，
---

# Lightcone Relayer

### 光锥中继JSON RPC接口标准

光锥中继JSON RPC接口遵循路印协议JSON RPC接口标准。所有的JSON PRC接口参数如下：

{% code-tabs %}
{% code-tabs-item title="JSON-RPC 接口请求模板" %}
```text
{
  "jsonrpc": "2.0", // required, must be "2.0"
  "method": "method_name", // required, the method to be invoked
  "id": 1, // required, a unique id for each invocation
  "params": {} // optional, the method's parameter object
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

所有的错误返回遵循以下的结构：

{% code-tabs %}
{% code-tabs-item title="光锥中继失败的返回模板" %}
```text

{
  "id": 1,
  "jsonrpc": "2.0",
  "error": {
    "code": -32603,
    "message": ""
  }

```
{% endcode-tabs-item %}
{% endcode-tabs %}

## JSON RPC 接口

光锥中继实现了所有路印协议Relay基础接口，详情见路印协议[Standard API](https://docs.loopring.org/~/edit/drafts/-LXHb7K0KYpTs2dW1Cig/relayer/apis)。

###  get\_token\_metadata

 获取Relay支持交易的token的信息，包括最小下单量，最大下单量，保留小数位，地址, symbol,decimal等

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "jsonrpc": "2.0",
  "method": "loopring_getTokenMetaData",
  "params": {tokens:["LRC","WETH"]},
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="返回样例" %}
```text
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
{% endcode-tabs-item %}
{% endcode-tabs %}

### get\_balances

获取指定用户指定token的余额和授权信息

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "jsonrpc": "2.0",
  "method": "loopring_getAssets",
  "params": {
    "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
    "tokens": [   //token 可以是token的名称或者地址
      "LRC",
      "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
    ]
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="返回样例" %}
```text
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
       {
        "symbol": "WETH",
        "balance": "0x1326beb03e0a0000",
        "allowance": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
      },
      ...
    ]
  }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### get\_trades

获取用户的成交记录

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_trades",
  "params": {
      "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
      "market": "LRC-WETH",
      "pageNum": 1,
      "pageSize": 50
    },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="返回样例" %}
```text
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
{% endcode-tabs-item %}
{% endcode-tabs %}

### get\_orderBook

获取order book

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_orderBook",
  "params": {
      "level": 0,
      "size": 100,
      "market": "LRC-WETH"
    },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="返回样例" %}
```text
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "lastPrice": 0.0007,
    "sells": [
      {
        "amount": 1000,
        "price": 0.0007,
        "total": 0.7
      },
      ...
    ],
    "buys": [
      {
        "amount": 1000,
        "price": 0.00069,
        "total": 0.69
      },
      ...
    ]
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### get\_ticker

获取指定交易对的Ticker信息

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "jsonrpc": "2.0",
  "method": "loopring_getTikcer",
  "params": {
    "market": "LRC-WETH"
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="返回样例" %}
```text
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "high": 30384.2,
    "low": 19283.2,
    "last": 28002.2,
    "vol": 1038,
    "amount": 1003839.32,
    "buy": 122321,
    "sell": 12388,
    "change": "50.12%"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### get\_transaction\_count

获取指定地址的交易数量

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "jsonrpc": "2.0",
  "method": "loopring_getTransactionCount",
  "params": {
    "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
    "tag": "latest"
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="返回样例" %}
```text
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x10" // 16
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}



