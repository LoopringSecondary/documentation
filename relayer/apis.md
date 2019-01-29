# Standard API

## Loopring Relayer Standard JSON RPC

All Loopring relayers shall implement the following standard JSON RPC. All RPC share the same endpoint and only use the HTTP POST method.

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

> The request supports the following parameters:
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

> The response contains the following fields:
>
> * **jsonrpc**: this filed will always have "2.0" as the value.
> * **id**: this field will have the same value as presented in the request.
> * **result**: this filed is the returned value or object.

{% code-tabs %}
{% code-tabs-item title="JSON RPC Error Response Template" %}
```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": -32603,
    "message": "some readable message"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> The error response contains the following fields:
>
> * **jsonrpc**: this filed will always have "2.0" as the value.
> * **id**: this field will have the same value as presented in the request.
> * **error**: the error object which has two fields:
>   * code: required, the integer error code. For a list of error code, please see the Error Code section below.
>   * message: optional, a readable message.

## JSON RPCS

### get\_time

 Get the relayer's time in millisecond.

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "get_time"
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id":1,
  "result": 1548410119809 // timestamp in milisecond
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### get\_markets

Get a list of markets supported by the relayer and their metadata.

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "get_markets",
  "params": {
    "status": "active"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> The **params** object supports the following parameters:
>
> * **status**: optional, the status of the markets to be retrieved, can be either "active" or "suspended", defaults to "active". When use the default value, the entire **params** object can be omitted.

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "markets" = [{
      "id": "LRC-WETH",
      "precisionForAmount": 5,
      "precisionForTotal": 8,
      "precisionForPrice": 5
    },
    ...]
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> The **result** object contains the following fields:
>
> * **markets**: a list of markets and their metadata.
>
> Each market has the following metadata:
>
> * **symbol**: the id of the market.
> * **status**: the status of the market, "active" or "suspended"
> * **precisionFormAmount**:
> * **precisionForTotal**:
> * **precisionForPrice**:

### get\_tokens

 Get the list of tokens and their metadata related to the markets supported by this relayer.

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "get_tokens",
  "params": {
  tokens:["LRC","WETH"]}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "tokens": [
      {
        "symbol": "LRC",
        "minTradeAmount": 0,
        "maxTradeAmount": 100000000,
        "precision": 5,
        "decimal": 18,
        "address": "0xef68e7c694f40c8202821edf525de3782458639f"
      },
      {  
        "symbol": "WETH",
        "minTradeAmount": 0,
        "maxTradeAmount": 100000000,
        "precision": 5,
        "decimal": 18,
        "address": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"  
      }
    ]
  }
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
  "id": 1,
  "method": "submit_order",
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

> The **params** object is a signed Loopring order. 
>
> Please refer to JSON Schema for more information regarding the order structure and how orders are signed.

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id":1,
  "result":"0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9" 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> * **result**: the hash \(id\) of the order that has been submitted. Please refer to JSON Schema for more information regarding how order hash \(id\) is calculated.

### cance\_order

Soft-cancel an order. This will not create a on-chain order cancellation transaction.

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "cancel_order",
  "params": {
   "orderHash":"0xfff19b90ccf6eb051dd71aa29350acbcaedbbfb841e66c202fda5bf7bd084b85"
   "timestamp":1548654606,
   "sig":"0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> * **orderHash**: required, the hash of the order to be soft-cancelled.
> * **timestamp**: required, the current timestamp.
> * **sig**: required, the signature of {orderHash,  timestamp} using this order's owner private key. Please refer to JSON Schema for how this request is signed.
>
> A cancellation request is valid only when the difference between relayer's system time and the timestamp parameter value is not greater than 1 minute.

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id":1,
  "result": "0xfff19b90ccf6eb051dd71aa29350acbcaedbbfb841e66c202fda5bf7bd084b85"
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> * **result**: the hash of the order that has been cancelled.

### cancel\_orders

Cancel all orders for a trading pair.

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "cancel_orders",
  "params": {
    "market": "LRC-WETH",
    "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
    "timestamp": 1548654606,
    "sig": "0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> The **params** object supports the following parameters:
>
> * **market**: The target market to cancel all orders. the market id must be in uppercase.
> * **owner**: the address whose orders will be cancelled.
> * **timestamp**: required, the current timestamp.
> * **sig**: required, the signature of {orderHash,  timestamp} using this order's owner private key. Please refer to JSON Schema for how this request is signed.
>
> A cancellation request is valid only when the difference between relayer's system time and the timestamp parameter value is not greater than 1 minute.

{% code-tabs %}
{% code-tabs-item title="Resonse Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id":1,
  "result": 20 //代表被取消的订单数量
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> * **result**: the number of orders that have been cancelled.

### get\_order

Get  orders based on their hashes.

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "get_orders",
  "params": {
    "order_hash": "0xfff19b90ccf6eb051dd71aa29350acbcaedbbfb841e66c202fda5bf7bd084b85"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> The **params** object supports the following parameters:
>
> * **order\_hash**: the hash of the order to be retrieved.

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "order": {
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

> The **result** object contains the following fields:
>
> * **order**: the order retrieved. If no order is found for a hash, an error object will be returned instead.
>
> The order's Dual Authoring private key will not be returned.

### get\_orders

Get a list of orders.

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "get_orders",
  "params": {
    "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
    "markets": ["LRC-WETH"],
    "statuses": ["0x0"],
    "pageNum": 1,
    "pageSize": 50
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> The **params** object supports the following parameters:
>
> * **owner**: required, the owing address of orders.
> * **markets**: optional, the list of markets from which orders are retrieved. If this value is omitted, orders from all markets will be retrieved.
> * **statuses**:  optional, the list of order statuses to filter. If this value is omitted, orders of all possible status will be retrieved. Please refer to JSON Schema for a list of order status.
> * **pageNum**: optional, the page number. The first page is 1, not 0, defaults to 1.
> * **pageSize**: optional, the number of orders per page, must be in the range of 10-100, inclusive. Defaults to 20.

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```javascript
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "pageNum": 1,
    "pageSize": 50,
    "total": 60,
    "orders": [
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
      },
      ...
    ]
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> The **result** object contains the following fields:
>
> * **pageNum**: the current page number \(same as in the request\).
> * **pageSize**: the size of the page \(same as in the request\).
> * **total**: the total number of orders available.
> * **orders**: the list of orders requested. Orders are sorted by the timestamp of their submission to the relayer.
>
> The Dual Authoring private keys will not be returned.



