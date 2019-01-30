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
    "tokens": [
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
        "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
        "orderHash": "",
        "ringHash": "",
        "ringIndex": "0x1",
        "fillIndex": "0x0",
        "txHash": "0x9ab523ac966a375f02c5b22e275a6e4c9c621f83881650587bc331e95ee5e73",
        "amountS": "0x8ac7230489e80000",
        "amountB": "0xde0b6b3a7640000",
        "tokenS": "0xef68e7c694f40c8202821edf525de3782458639f",
        "tokenB": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
        "marketKey": "",
        "split": "0xde0b6b3a7640000",
        "fee": {
          "tokenFee": "0xef68e7c694f40c8202821edf525de3782458639f",
          "amountFee": "0x2",
          "feeAmountS": "0x0",
          "feeAmountB": "0x0",
          "feeRecipient": "0xb94065482ad64d4c2b9252358d746b39e820a582",
          "waiveFeePercentage": "0x0",
          "walletSplitPercentage": "0x28"
        },
        "wallet": "0xb94065482ad64d4c2b9252358d746b39e820a582",
        "miner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
        "blockHeight": "0x8",
        "blockTimestamp": "0x5c4add07",
        
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
* **trades**: a list of trades.  Please refer to JSON Schema for more information regarding the [trade structure ](https://docs.loopring.org/~/drafts/-LXSU8n477Z_LHmNNqUj/primary/relayer/json-schema#trade-structure)

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

The **params** contains the following parameters:

* market : the market symbol of ticker to be retrieved

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

The **result**  contains the following fields:

* high:  the highest price for past 24 hours
* low: the lowest price for past 24 hours
* last: the latest trade price
* vol: the volume for past 24 hours
* amount: the amount of base token traded
* buy: the total buy amount
* sell: the total sell amount
* change: the change rate for past 24 hours

### get\_rings

retrieve rings

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_rings",
  "params": {
    "ringHash": "0xb94065482ad64d4c2b9252358d746b39e820a582",
    "ringIndex": "0x0",
    "sort": "asc",
    "pageNum": 1,
    "pageSize": 50
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The **Params** supports the following parameters:

* **ringHash**: optional,  hash of the ring to be retrieved
* **ringIndex**: optional,  index of the ring to be retrieved
* **sort**: optional, can be "asc" or "desc", default is "asc"
* **pageNum:** optional, the page number. The first page is 1, not 0, defaults to 1.
*  **pageSize :** optional, the number of orders per page, must be in the range of 10-100, inclusive. Defaults to 20get\_transaction\_count

```text
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "pageNum": 1,
    "pageSize": 50,
    "total": 40,
    "rings": [
      "ringHash": "",
      "ringIndex": "0x1",
      "fillsAmount": "0x2",
      "miner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
      "txHash": "0x9ab523ac966a375f02c5b22e275a6e4c9c621f83881650587bc331e95ee5e73",
      "fees": [
        {
          "tokenFee": "0xef68e7c694f40c8202821edf525de3782458639f",
          "amountFee": "0x2",
          "feeAmountS": "0x0",
          "feeAmountB": "0x0",
          "feeRecipient": "0xb94065482ad64d4c2b9252358d746b39e820a582",
          "waiveFeePercentage": "0x0",
          "walletSplitPercentage": "0x28"
        }
      ],
      blockHeight: "0x8",
      blockTimestamp: "0x5c4add07"
    ]
  }
}
```

**Result** contains the following fields :

* pageNum
* pageSize
* total
* rings: a list of rings. Please refer to JSON Schema for more information regarding the [ring structure ](https://docs.loopring.org/~/drafts/-LXSU8n477Z_LHmNNqUj/primary/relayer/json-schema#ring-structure)

get the transaction count of given address

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
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

The **params** contains the following parameters:

* owner:  the address of transaction count to be retrieved.
* tag:  
  * "latest" :  only contains the transactions only have mined in blocks
  * "pending":  also contains pending transactions

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
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

get the transfer record of given owner

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
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

The **params** contains the following parameters:

* tokens: a list of token address
* type
  *  "0x0"   ：income
  *  "0x1"  ： outcome
* status
  * "0x0"  : succeed
  * "0x1" : failed 
  *  "0x2" : pending
* pageNum: optional, the page number. The first page is 1, not 0, defaults to 1.
* pageSize: optional, the number of orders per page, must be in the range of 10-100, inclusive. Defaults to 20.

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```text
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "pageNum": 1,
    "pageSize": 50,
    "total": 60",
    "tranfers": [
      {
        "from": "0xb94065482ad64d4c2b9252358d746b39e820a582",
        "to": "0x6cc5f688a315f3dc28a7781717a9a798a59fda7b",
        "token": "LRC",
        "amount": "1000.0000",
        "txHash": "0x9ab523ac966a375f02c5b22e275a6e4c9c621f83881650587bc331e895ee5e73",
        "time": "0x5c4add07",
        "status": "0x0"
      },
      ...
    ]
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The **result** contains the following result:

* transfers :  a list of  transfers. 

Each transfer contains the following result:

* from:  the sender of transfer
* to: the receiver of transfer
* token: token that the sender sends to the receiver
* amount: the amount that the sender sends to the receiver
* txHash:  tx hash 
* time : block time
* status: the status of the transaction 

**get\_transactions**

get the transactions of given address

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
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

The **params** contains the following parameters:

* owner: the owner address of transactions to be retrieved
* status:
  * "0x0"  :  succeed
  * "0x1" : failed 
  *  "0x2" : pending
* type:
  * "0x0"  : eth transfer
  * "0x1"  : erc20 token transfer
  * "0x2"  : cancel order
  * "0x3" : ring mined

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
    "transctions": [
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

The **result** contains the following fields:

* transactions: a list of transactions. 

each transaction contained the following fields:

* from : the sender address of the transaction.
* to : the receiver address of the transaction
* value: the ethereum transaction value.
* gasPrice: the ethereum transaction gas price
* gasLimit: the ethereum transaction gas limit
* gasUsed: the actual gas that used
* data : ethereum transaction input
* nonce:  the ethereum transaction nonce
* hash: transaction hash
* blockNum: the blockNum that the transaction mined in
* time : the block time
* status

## Socket 

### balances

Subscriber balance and allowance of given address

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
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

 About the  request **params** please refer to the JSON RPC interface named "get\_balances"

{% code-tabs %}
{% code-tabs-item title="Response Exmaple" %}
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

About the **result** please refer to the JSON RPC interface named "get\_balances"

### **orders**

Subscriber orders of given owner

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
  "markets": ["LRC-WETH"],
  "statuses": ["0x0"]
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

About the request **params,** please refer to the JOSN RPC interface named "get\_orders"

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
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

The **result** is a list of orders.  Please refer to the [JSON Schema](https://docs.loopring.org/~/drafts/-LXHyMXc89BbDx77pi4r/primary/relayer/json-schema#order-ding-dan-jie-gou) for the definition of a Loopring order.

### trades

subscriber trades of given trades

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
  "markets": ["LRC-WETH"]
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

About the request **params**, please refer to the JSON RPC interface named "get\_trade"

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
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

About the detail of the result , please refer to the JOSN RPC interface named "get\_trade"

### order\_book

Subscriber order book of given market

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{"level":0,"size":100,"market":"LRC-WETH"}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

About the request **params** object , please refer to JSON RPC interface named:"get\_order\_book"

{% code-tabs %}
{% code-tabs-item title="Response Exmaple" %}
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

About the detail of the result , please refer to the JOSN RPC interface named "get\_order\_book"

### **ticker**

Subscriber ticker

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{"market":"LRC-WETH"}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The request **params** contains the following parameters:

* market : the market symbol of ticker to be retrieved

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
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

please refer to the result of JSON RPC interface named "get\_ticker"

**transfers**

Subscriber the transfer  

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{"tokens":["LRC","WETH"],"type":"0x0","status":"0x0"}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

please refer to the **Params** of JSON RPC interface named "get\_transfer"

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
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

please refer to the **result** of the JOSN RPC interface named "get\_transfer"

### transactions

Subscriber  the ethereum transactions

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{"owner":"0xb94065482ad64d4c2b9252358d746b39e820a582","status":"0x0"}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

about the detail request parameter, please refer to the JSON RPC interface named "get\_transaction"

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
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

About the detail of  the result,  please refer to the JSON RPC interface named "get\_transaction"

