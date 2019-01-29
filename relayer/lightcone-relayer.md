# Lightcone Relayer

### Lightcone Relayer JSON RPC

The Lightcone relayer implements all Loopring Relayer Standard JSON RPC API, as well as a set of additional JSON RPC API described in this document. The Lightcone relayer also implements [all Ethereum JSON RPC API](https://github.com/ethereum/wiki/wiki/JSON-RPC) as well.

All RPC share the same endpoint and only use the HTTP POST method.

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

## Lightcone's Additional JSON RPC

### get\_balances

get the balances an allowances of given tokens and owner

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_balances",
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

The params object support the following parameters:

* owner : the owner address 
* tokens : a list of tokens, token can token's address or token's symbol

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```text
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
    "balanceAndAllowances": [
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

The **result** object contains the following fields:

> * **owner** : the given owner address .
> * **balanceAndAllowances**: a list of given owner's balances and allowances
>
> each balanceAndAllowance contains the following fields:
>
> * **symbol** : token symbol
> * **balance** : balance  in hex string
> * **allowance**: allowance in hex string

### get\_trades

get the given owner's trade record according to given query conditions.

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_trades",
  "params": {
      "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
      "markets": ["LRC-WETH"],
      "pageNum": 1,
      "pageSize": 50
    },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The **params** object support the following parameters:

* **owner** :  owner address
* **markets**:   optional, the list of markets from which orders are retrieved. If this value is omitted, orders from all markets will be retrieved.
* **pageNum**: optional, the page number. The first page is 1, not 0, defaults to 1.
* **pageSize** : optional, the number of orders per page, must be in the range of 10-100, inclusive. Defaults to 20.

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```text
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "pageNum": 1,
    "pageSize": 50,
    "total": 60,
    "trades": [
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

The **result** object contains the following fields:

* **pageNum**
* **pageSize**
* **total**
* **trades**: a list of trades.  Please refer to JSON Schema for more information regarding the trade structure 

### get\_order\_book

get order book of given market 

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_order_book",
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

The **params** object support the following parameters:

* **level** : the precision level of order book.
* **size** : the max size of each sells and buys.
* **market**: market symbol of order book to be retrieved.

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
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

The **result** object contains the following fields:

* **lastPrice**: the latest trade price
* **buys**:  a list of buys.
* **sells** : a list of sells.

each buy or sell contains  the following fields:

* **amount**
* **price**
* **total**

### get\_ticker

get the ticker of given market 

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_tikcer",
  "params": {
    "market": "LRC-WETH"
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The **Params** contains the 

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
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
  "method": "get_transaction_count",
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

**get\_transfers**

获取用户的转账记录

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_transfers",
  "params": {
    "tokens": [
      "LRC",
      "WETH"
    ],
    "type": "0x0",
    "status": "0x0",
    "pageNum": 1,
    "pageSize": 50
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

* type
  *  "0x0"   ：转入
  *  "0x1"  ： 转出
* status
  * "0x0"  : 成功
  * "0x1" : 失败 
  *  "0x2" : pending

{% code-tabs %}
{% code-tabs-item title="返回样例" %}
```text
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "pageNum": 1,
    "pageSize": 50,
    "total": 60"records": [
      {
        "from": "0xb94065482ad64d4c2b9252358d746b39e820a582",
        "to": "0x6cc5f688a315f3dc28a7781717a9a798a59fda7b",
        "token": "LRC",
        "amount": "1000.0000",
        "txHash": "0x9ab523ac966a375f02c5b22e275a6e4c9c621f83881650587bc331e895ee5e73",
        "time": "0x5c4add07",
        "status": 0
      },
      ...
    ]
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**get\_transactions**

获取用户的以太坊交易

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_transactions",
  "params": [
    {
      "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
      "status": "0x0",
      "type": "0x0",
      "pageNum": 1,
      "pageSize": 50
    }
  ],
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

type :

* "0x0"   ETH 转账
* "0x1"   ERC20 转账
* "0x2"   cancel
* "0x3"   撮合订单

status:

* "0x0"  : 成功
* "0x1" : 失败 
*  "0x2" : pending

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
        "from": "0xb94065482ad64d4c2b9252358d746b39e820a582",
        "to": "0x6cc5f688a315f3dc28a7781717a9a798a59fda7b",
        "value": "1000.0000",
        "gasPrice": "0x2540be400",
        "gasLimit": "0x5208",
        "gasUsed": "0x5208",
        "data": "0x",
        "nonce": "0x9",
        "hash": "0x9ab523ac966a375f02c5b22e275a6e4c9c621f83881650587bc331e95ee5e73",
        "blockNum": "0x6cc501",
        "time": "0x5c4add07",
        "status": 0
      },
      ...
    ]
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Socket 接口

### balances

订阅用户的余额和授权

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
  "tokens": [
    "LRC",
    "WETH"
  ]
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="返回样例" %}
```text
[
  {
    "symbol": "LRC",
    "balance": "0x1326beb03e0a0000",
    "allowance": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
  },
  ...
]
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### orders

订阅用户的订单

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
  "market": "LRC-WETH",
  "status": "0x0"
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="返回样例" %}
```text
[
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
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### trades

订阅用户的交易记录

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{
  "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
  "market": "LRC-WETH"
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="返回样例" %}
```text
[
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
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### order\_book

订阅Order Book

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{"level":0,"size":100,"market":"LRC-WETH"}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="返回样例" %}
```text
{
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
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**ticker**

订阅ticker

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{"market":"LRC-WETH"}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="返回样例" %}
```text
{
    "high" : 30384.2,
    "low" : 19283.2,
    "last" : 28002.2,
    "vol" : 1038,
    "amount" : 1003839.32,
    "buy" : 122321,
    "sell" : 12388,
    "change" : "50.12%"
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**transfers**

订阅用户的转账记录

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{"tokens":["LRC","WETH"],"type":"0x0","status":"0x0"}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

* type
  *  "0x0"   ：转入
  *  "0x1"  ： 转出
* status
  * "0x0"  : 成功
  * "0x1" : 失败 
  *  "0x2" : pending

{% code-tabs %}
{% code-tabs-item title="返回样例" %}
```text
[
  {
    "from": "0xb94065482ad64d4c2b9252358d746b39e820a582",
    "to": "0x6cc5f688a315f3dc28a7781717a9a798a59fda7b",
    "token": "LRC",
    "amount": "1000.0000",
    "txHash": "0x9ab523ac966a375f02c5b22e275a6e4c9c621f83881650587bc331e895ee5e73",
    "time": "0x5c4add07",
    "status": 0
  },
  ...
]
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### transactions

订阅用户的以太坊交易

{% code-tabs %}
{% code-tabs-item title="请求样例" %}
```text
{"owner":"0xb94065482ad64d4c2b9252358d746b39e820a582","status":"0x0"}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

status :

* "0x0"   ETH 转账
* "0x1"   ERC20 转账
* "0x2"   cancel
* "0x3"   撮合订单

{% code-tabs %}
{% code-tabs-item title="返回样例" %}
```text
[
  {
    "from": "0xb94065482ad64d4c2b9252358d746b39e820a582",
    "to": "0x6cc5f688a315f3dc28a7781717a9a798a59fda7b",
    "value": "1000.0000",
    "gasPrice": "0x2540be400",
    "gasLimit": "0x5208",
    "gasUsed": "0x5208",
    "data": "0x",
    "nonce": "0x9",
    "hash": "0x9ab523ac966a375f02c5b22e275a6e4c9c621f83881650587bc331e95ee5e73",
    "blockNum": "0x6cc501",
    "time": "0x5c4add07",
    "status": 0
  },
  ...
]
```
{% endcode-tabs-item %}
{% endcode-tabs %}

