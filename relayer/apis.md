# Standard API

## Loopring Relayer Standard JSON RPC

All Loopring relayer shall implement the following standard JSON RPC. All RPC share the same endpoint and only use the HTTP POST method.

All JSON RPC shares the following request parameters:

{% code-tabs %}
{% code-tabs-item title="JSON RPC Request Template" %}
```javascript
{
  "jsonrpc": "2.0",
  "method": "method_name",
  "id": 1, 
  "params": {}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> The following fields are supported in the request object:
>
> * **jsonrpc**: required, and its value must be "2.0".
> * **method**: required, the method to be invoked.
> * **id**: required: a random integer number associated with this request.
> * **params**: optional, for come JSON RPC, this is the object containing RPC specific parameters.

{% code-tabs %}
{% code-tabs-item title="JSON RPC Response Template" %}
```javascript
{
  "jsonrpc": "2.0",
  "id":1,
  "result": {}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> The response object contains the following fields:
>
> * **jsonrpc**: this filed will always have "2.0" as the value.
> * **id**: this field will have the same value as presented in the request.
> * **result**: this filed is the returned value or object.

{% code-tabs %}
{% code-tabs-item title="JSON RPC Error Response Template" %}
```javascript
{
  "error"...
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## JSON RPCS

### get\_time

 Get the relayer's time in millisecond.

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "method": "get_time",
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```javascript
{
  "id":1,
  "jsonrpc": "2.0",
  "result": 1548410119809 // timestamp in milisecond
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### submit\_order

Submit a limit price order.  Please refer to the [JSON Schema](https://docs.loopring.org/~/drafts/-LXHyMXc89BbDx77pi4r/primary/relayer/json-schema#order-ding-dan-jie-gou) for the definition of a Loopring order.

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "method": "submit_order",
  "id": 1,
  "params": {
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
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> The params object represent a signed Loopring order. For more information regarding the order data structure and order signing, please refer JSON Schema.

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```javascript
  {
    "id":1,
    "jsonrpc": "2.0",
    "result":"0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9" 
  }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> The result is the hash \(id\) of the order that has been submitted. Please refer to JSON Schema for how the order's hash is calculated.

### cance\_order

Soft-cancel an order. This will not create a on-chain order cancellation transaction.

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "method": "cancel_order",
  "params": {
   "orderHash":"0xfff19b90ccf6eb051dd71aa29350acbcaedbbfb841e66c202fda5bf7bd084b85"
   "timestamp":1548654606,
   "sig":"0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b"
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> * **orderHash**: required, the hash of the order to be soft-cancelled.
> * **timestamp**: required, the current timestamp.
> * **sig**: required, the signature of {orderHash,  timestamp} using this order's owner private key. Please refer to JSON Schema for how this request is signed.
>
> Note that such a cancellation request is considered valid only when the difference between relayer's system time and the timestamp parameter value is not greater than 1 minute.

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```javascript
{
  "id":1,
  "jsonrpc": "2.0",
  "result":"0xfff19b90ccf6eb051dd71aa29350acbcaedbbfb841e66c202fda5bf7bd084b85"
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> * **result**: the  hash of the order that has been cancelled.

### cancel\_orders

Cancel all orders for a trading pair.

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```javascript
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

> The requests contains the following parameters:
>
> * **market**: The target market to cancel all orders. the market id must be in uppercase.
> * **owner**: the address whose orders will be cancelled.
> * **timestamp**: required, the current timestamp.
> * **sig**: required, the signature of {orderHash,  timestamp} using this order's owner private key. Please refer to JSON Schema for how this request is signed.
>
> Note that such a cancellation request is considered valid only when the difference between relayer's system time and the timestamp parameter value is not greater than 1 minute.

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

## Error Codes



