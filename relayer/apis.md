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
  "result": "0x168846f6681" // timestamp in milisecond
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
    "requireMetadata": true,
    "requireTicker": true,
    "quoteCurrencyForTicker": "USD" 
    "marketPairs": [
      
    ]
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

* currently support for "USD","RMB"

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "markets"=[
      {
        "metadata": {
          "status": "ACTIVE",
          "priceDecimals": 5,
          "orderbookAggLevels": 3,
          "precisionForAmount": 1,
          "precisionForTotal": 5,
          "browsableInWallet": true,
          "marketPair": {
            "baseToken": "0xef68e7c694f40c8202821edf525de3782458639f",
            "quoteToken": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
          },
          "marketHash": "0x2f424dff26d7f20f088c429075b73a70182d0f5d"
        },
        "ticker": {
          "baseToken": "0xef68e7c694f40c8202821edf525de3782458639f",
          "quoteToken": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
          "exchangeRate": 0.000485,
          "price": 0.05,
          "volume24H": 1234.5,
          "percentChange1H": 5.121,
          "percentChange24H": 4.567,
          "percentChange7D": 60.212
        }
      }
    ]
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> The **result** object contains the following fields:
>
> * **markets**: a list of markets and their metadata.
>
>  Market  status can be the following values:

> * **status**: the status of the market, "ACTIVE" 、 "READONLY"、"TERMINATED"

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
    "requireMetadata": true,
    "requireInfo": ture,
    "requirePrice": true,
    "quoteCurrencyForPrice": "USD",
    "tokens": [
      
    ]
  }
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
        "metadata": {
          "type": "TOKEN_TYPE_ERC20",
          "status": "VALID",
          "symbol": "LRC",
          "name": "Loopring Token",
          "address": "0xef68e7c694f40c8202821edf525de3782458639f",
          "unit": "LRC",
          "decimals": 18,
          "precision": 6,
          "burnRate": {
            "forMarket": 0.5,
            "forP2P": 0.4
          }
        },
        "info": {
          symbol: "LRC",
          circulatingSupply: 800000000,
          totalSupply: 1400000000,
          maxSupply: 1400000000,
          cmcRank: 80,
          icoRateWithEth: 0.00016,
          websiteUrl: "www.loopring.io"
        },
        "ticker": {
          token: "0xef68e7c694f40c8202821edf525de3782458639f",
          price: 0.00048,
          volume24H: 12000000,
          percentChange1H: 2.34,
          percentChange24H: -1.2,
          percentChange7D: 102.45
        }
      }
    }
  }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The **result** object contains the following fields:

> * **tokens**: a list of tokens and their metadata.
>
> Each token contains the following fields:
>
> * metadata
> * info
> * ticker

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
  "id": 1,
  "result": {
    "success": true
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### cancel\_orders

Cancel  order for a given id, or orders for given market or all orders of given owner

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "cancel_orders",
  "params": {
    "id": "",
    "marketPair": {
      "baseToken": "0xef68e7c694f40c8202821edf525de3782458639f",
      "quoteToken": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
    },
    "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
    "time": "0x5c4e980e",
    "sig": "0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> The **params** object supports the following parameters:
>
> * **id:** the order hash of the order to cancel
> * **market**: The target market to cancel all orders. the market id must be in uppercase.
> * **owner**: the address whose orders will be cancelled.
> * **time**: required, the current timestamp.
> * **sig**: required, the signature  using this order's owner private key in EIP 712  way. Please refer to JSON Schema for how this request is signed.
>
> A cancellation request is valid only when the difference between relayer's system time and the timestamp parameter value is not greater than 1 minute.

{% code-tabs %}
{% code-tabs-item title="Resonse Example" %}
```javascript
{
  "jsonrpc": "2.0",
  "id":1,
  "result": {
  "status":"STATUS_SOFT_CANCELLED_BY_USER"
  } 
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> * **result**: the final status of the orders be canceled

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
    "market": {
        "baseToken": "0xef68e7c694f40c8202821edf525de3782458639f",
        "quoteToken": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
      },
    "statuses": [
      "0x0"
    ],
    "sort":"asc",
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
> * **market**: optional, the market from which orders are retrieved. If this value is omitted, orders from all markets will be retrieved.
> * **statuses**:  optional, the list of order statuses to filter. If this value is omitted, orders of all possible status will be retrieved. Please refer to JSON Schema for a list of order status.
> * sort: "asc" or "desc"
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



