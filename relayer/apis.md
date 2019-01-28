---
description: 路印光锥中继API文档
---

# Standard API

## Loopring Relayer Standard JSON RPC

All Loopring relayer shall implement the following standard JSON RPC. All RPC share the same endpoint and only use the HTTP POST method.

All JSON RPC shares the following request parameters:

{% code-tabs %}
{% code-tabs-item title="JSON RPC Request Template" %}
```javascript
{
  "jsonrpc": "2.0", // required, must be "2.0"
  "method": "method_name", // required, the method to be invoked
  "id": 1, // required, a unique id for each invocation
  "params": {} // optional, the method's parameter object
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### get\_time

 获取server的时间，单位是毫秒

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_time",
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
  "result": 1548410119809
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### submit\_order

用户提交订单, 订单结构可以参考路印协议[JSON Scheam](https://docs.loopring.org/~/drafts/-LXHyMXc89BbDx77pi4r/primary/relayer/json-schema#order-ding-dan-jie-gou) 订单结构

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "jsonrpc": "2.0",
  "method": "submit_order",
  "params": [
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
  ],
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
    "result":"0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9"
  }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### cance\_order

用户取消订单

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "jsonrpc": "2.0",
  "method": "cancel_order",
  "params": {
   "orderHash":"0xfff19b90ccf6eb051dd71aa29350acbcaedbbfb841e66c202fda5bf7bd084b85"
   "timestamp":1548654606,
   "owner":"0xb94065482ad64d4c2b9252358d746b39e820a582",
   "sig":"0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b"
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
  "result":"0xfff19b90ccf6eb051dd71aa29350acbcaedbbfb841e66c202fda5bf7bd084b85"
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### cancel\_orders

取消指定用户指定市场对的全部订单，如果没有指定市场则取消指定用户的全部订单。

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "jsonrpc": "2.0",
  "method": "cancel_orders",
  "params": {
    "market": "LRC-WETH",
    "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
    "timestamp": 1548654606,
    "sig": "0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b"
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
  "result": 20 //代表被取消的订单数量
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### get\_orders

获取用户的订单列表

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_orders",
  "params": [
    {
      "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
      "market": "LRC-WETH",
      "status": "0x0",
      "pageNum": 1,
      "pageSize": 50
    }
  ],
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### status 

* "0x0" 代表pending
* "0x1" 代表撮合完成
* "0x2"代表取消
* "0x3"代表失效过期的

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
            "dualAuthAddr": "0x7ebdf3751f63a5fc1742ba98ee34392ce82fa8dd"
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
{% endcode-tabs-item %}
{% endcode-tabs %}

### get\_order

根据OrderHash 查询单条Order记录

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_order",
  "params": [
    "0xfff19b90ccf6eb051dd71aa29350acbcaedbbfb841e66c202fda5bf7bd084b85"
  ],
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
        "dualAuthAddr": "0x7ebdf3751f63a5fc1742ba98ee34392ce82fa8dd"
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
{% endcode-tabs-item %}
{% endcode-tabs %}

### get\_market\_meta\_data

获取relay支持的市场信息

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_market_meta_data",
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
{% endcode-tabs-item %}
{% endcode-tabs %}

