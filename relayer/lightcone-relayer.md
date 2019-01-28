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

