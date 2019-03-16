# Lightcone Relayer

### Lightcone Relayer JSON RPC

The Lightcone relayer implements all Loopring Relayer Standard JSON RPC API, as well as a set of additional JSON RPC API described in this document. The Lightcone relayer also implements all [Ethereum JSON RPC API](https://github.com/ethereum/wiki/wiki/JSON-RPC) as well.

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

### get\_accounts

get the balances an allowances of given tokens and addresses

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_accounts",
  "params": {
    "addresses": [
      "0xb94065482ad64d4c2b9252358d746b39e820a582"
    ],
    "tokens": [
      
    ],
    "allTokens": true
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The params object support the following parameters:

* addresses : the owner addresses 
* tokens : a list of token addresses
* allTokens: if query all the tokens lightcone supports

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```text
{
  "jsonrpc": "2.0",
  "result": {
    "accountBalances": {
      "0xe20cf871f1646d8651ee9dc95aab1d93160b3467": {
        "address": "0xe20cf871f1646d8651ee9dc95aab1d93160b3467",
        "tokenBalanceMap": {
          "0x97241525fe425c90ebe5a41127816dcfa5954b06": {
            "token": "0x97241525fe425c90ebe5a41127816dcfa5954b06",
            "balance": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
            "allowance": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
            "availableBalance": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
            "availableAlloawnce": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
          },
          "0x7cb592d18d0c49751ba5fce76c1aec5bdd8941fc": {
            "token": "0x7cb592d18d0c49751ba5fce76c1aec5bdd8941fc",
            "balance": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
            "allowance": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
            "availableBalance": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
            "availableAlloawnce": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
          }
        }
      }
    }
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The **result** object contains the following fields:

> * **accountBalances**
>
> each accountBalance contains the following fields:
>
> * **address** : owner address
> * **tokenBalanceMap:** map of token balance map

### get\_account\_nonce

get the pending nonce of given address

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_trades",
  "params": {
    "address": "0xb94065482ad64d4c2b9252358d746b39e820a582"
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```text
{
  "jsonrpc": "2.0",
  "result": {
    "nonce": 44
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### get\_user\_fills

get the given owner's fill record according to given query conditions.

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_user_fills",
  "params": {
    "owner": "",
    "marketPair": {
      "baseToken": "0xef68e7c694f40c8202821edf525de3782458639f",
      "quoteToken": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
    },
    "sort": "ASC",
    "paging": {
      "skip": 0,
      "size": 50
    }
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The **params** object support the following parameters:

* **owner** :  owner address
* **marketPair**:   optional, the market from which orders are retrieved. If this value is omitted, orders from all markets will be retrieved.
* **sort**: "asc" or "desc".
* **paging**

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```text
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "total": 60,
    "fills": [
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

* **total**
* **fills**: a list of trades.  Please refer to JSON Schema for more information regarding the [trade structure ](https://docs.loopring.org/~/drafts/-LXSU8n477Z_LHmNNqUj/primary/relayer/json-schema#trade-structure)

### get\_market\_fills

query the latest 20 fills 

{% code-tabs %}
{% code-tabs-item title="Request  Example" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_market_fills",
  "params": {
    "marketPair": {
      "baseToken": "0xef68e7c694f40c8202821edf525de3782458639f",
      "quoteToken": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
    }
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```text
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "fills": [
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
        "blockTimestamp": "0x5c4add07"
      },
      ...
    ]
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

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
    "marketPair": {
      "baseToken": "0xef68e7c694f40c8202821edf525de3782458639f",
      "quoteToken": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
    }
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The **params** object support the following parameters:

* **level** : the precision level of order book.
* **size** : the max size of each sells and buys.
* **marketPair**: market symbol of order book to be retrieved.

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

### get\_rings

retrieve rings

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_rings",
  "params": {
    "sort": "ASC",
    "paging": {
      "skip": 300,
      "size": 50
    },
    "filter": {
      "ringIndex": 30
    }
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The **Params** supports the following parameters:

* **filter**
  * **ringHash**: optional,  hash of the ring to be retrieved
  * **ringIndex**: optional,  index of the ring to be retrieved
* **sort**: optional, can be "asc" or "desc", default is "asc"
* **paging**

```text
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
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

* **total**
* **rings**: a list of rings. Please refer to JSON Schema for more information regarding the [ring structure ](https://docs.loopring.org/~/drafts/-LXSU8n477Z_LHmNNqUj/primary/relayer/json-schema#ring-structure)

### get\_activities

get the activity records of given owner and token

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_activities",
  "params": {
    owner: "0xb94065482ad64d4c2b9252358d746b39e820a582",
    token: "0xef68e7c694f40c8202821edf525de3782458639f",
    paging: {
      "cursor": 100,
      "size": 50
    }
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The **params** contains the following parameters:

* **owner** 
* **token**  token address 
* **paging**

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```text
{
  "jsonrpc": "2.0",
  "result": {
    "activities": [
      {
        "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
        "block": 20,
        "txHash": "0x3c0c589480338327df0a941231df0b0267cf22dd9973770994df09e5f4965873",
        "activityType": "TOKEN_TRANSFER_IN",
        "timestamp": 1547782995,
        "fiatValue": 500,
        "token": "0xef68e7c694f40c8202821edf525de3782458639f",
        "from": "0x6cc5f688a315f3dc28a7781717a9a798a59fda7b",
        "nonce": 15,
        "txStatus": "TX_STATUS_PENDING",
        "detail": {
        "tokenTransfer":{
          "address": "0x6cc5f688a315f3dc28a7781717a9a798a59fda7b",
          "token": "0xef68e7c694f40c8202821edf525de3782458639f",
          "amount": "0x3635c9adc5dea00000"
          }
        }
      }
    ]
  },
  "id": 1
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The **result** contains the following result:

* activities :  a list of  activities. 

Each activity contains the following result:

* owner: the owner of the this activity
* block: block number 
* activityType: can be the following values:
  * ETHER\_TRANSFER\_OUT
  * ETHER\_TRANSFER\_IN 
  * ETHER\_WRAP
  * ETHER\_UNWRAP 
  * TOKEN\_TRANSFER\_OUT 
  * TOKEN\_TRANSFER\_IN 
  * TOKEN\_AUTH 
  * TRADE\_SELL 
  * TRADE\_BUY 
  * ORDER\_CANCEL 
  * ORDER\_SUBMIT
* timestamp : block time
* fiatValue
* token: token that the sender sends to the receiver
* nonce:  the transaction nonce
* txStatus: the status of the transaction, can be the following values:
  *  tx\_status\_pending
  *  tx\_status\_success
  *  tx\_status\_failed
* detail: according to activityType and can be the types
  * EtherTransferSocket ****
    * address 
    * amount
  * EtherConversion
    * amount
  * TokenTransfer
    * address // to or from
    * token
    * amount
  * TokenAuth
    * token
    * target 
    * amount
  * Trade
    * address
    * tokenBase
    * tokenQuote
    * price
    * amountBase
    * amountQuote
    * isP2p
  * OrderCancellation
    * orderIds
    * cutOff
    * marketPair
    * broker

### get\_market\_history

retrieve market trading history

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "jsonrpc": "2.0",
  "method": "get_market_history",
  "params": {
    "marketpair": {
      "baseToken": "0xef68e7c694f40c8202821edf525de3782458639f",
      "quoteToken": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
    },
    "interval": "OHLC_INTERVAL_ONE_WEEK",
    "beginTime": 1547782995,
    "endTime": 1552721131
  },
  "id": 1
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```text
{
  "jsonrpc": "2.0",
  "result": {
    "data": [
      {
        "data": [
          1547782995,
          350600.5,
          502.7,
          0.00048,
          0.00045,
          0.00049,
          0.000445
        ]
      }
    ]
  },
  "id": 1
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

the numbers  stands for  data as the following:

1. starting\_point 
2. quality amount 
3. opening\_price 
4. closing\_price
5.  highest\_price
6.  lowest\_price

## Socket

Lightcone socket support for subscribing activity, orders, fills, order book, metadata, internal ticker,  accounts and news.

subscribe Params contains paramsForActivities, paramsForOrder, paramsForFills, paramsForOrderbook, paramsForMetadata, paramsForInternalTickers, paramsForAccounts, paramsForNews

### paramsForAccounts

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "addresses": ["0xb94065482ad64d4c2b9252358d746b39e820a582"],
  "tokens": [
    "0xef68e7c694f40c8202821edf525de3782458639f",
    "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
  ]
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

 About the  request **params** please refer to the JSON RPC interface named "get\_balances"

{% code-tabs %}
{% code-tabs-item title="Response Exmaple" %}
```text
{
  "address": "0xb94065482ad64d4c2b9252358d746b39e820a582",
  "tokenBalance": {
    "token": "0xef68e7c694f40c8202821edf525de3782458639f",
    "balance": "0x1326beb03e0a0000",
    "allowance": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "block": 100
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### **paramsForOrders**

Subscriber orders of given addresses and marketPair

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "addresses": ["0xb94065482ad64d4c2b9252358d746b39e820a582"],
  "marketPair": {
    "baseToken": "0xef68e7c694f40c8202821edf525de3782458639f",
    "quoteToken": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

About the request **params,** please refer to the JOSN RPC interface named "get\_orders"

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```text
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
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The **result** is a list of orders.  Please refer to the [JSON Schema](https://docs.loopring.org/~/drafts/-LXHyMXc89BbDx77pi4r/primary/relayer/json-schema#order-ding-dan-jie-gou) for the definition of a Loopring order.

### paramsForFills

subscriber fills of given address and market

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "address": "0xb94065482ad64d4c2b9252358d746b39e820a582",
  "marketPair": {
    "baseToken": "0xef68e7c694f40c8202821edf525de3782458639f",
    "quoteToken": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```text
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
  "blockTimestamp": "0x5c4add07"
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### paramsForOrderbook

Subscribe order book of given market

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "level": 0,
  "marketPair": {
    "baseToken": "0xef68e7c694f40c8202821edf525de3782458639f",
    "quoteToken": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

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

### paramsForInternalTickers

Subscribe for given market internal ticker

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "marketpair": {
    "baseToken": "0xef68e7c694f40c8202821edf525de3782458639f",
    "quoteToken": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Response Example" %}
```text
{
  "ticker": {
    "baseToken": "0xef68e7c694f40c8202821edf525de3782458639f",
    "quoteToken": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
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

**transfers**

Subscriber the transfer  

{% code-tabs %}
{% code-tabs-item title="Request Example" %}
```text
{
  "owner": "0xb94065482ad64d4c2b9252358d746b39e820a582",
  "type": "income",
  "tokens": [
    "0xef68e7c694f40c8202821edf525de3782458639f",
    "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
  ]
}
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
    "status": "succeed"
    },
  ...
]
```
{% endcode-tabs-item %}
{% endcode-tabs %}

please refer to the **result** of the JOSN RPC interface named "get\_transfer"

