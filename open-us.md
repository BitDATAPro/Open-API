# BitDATA Open API

[Chinese](./open-en.md) | English

- [**Getting started**](#Getting-started)

  - [**Create API key**](#Create-API-key)
  - [**Interface call description**](#Description-on-the-way-of-calling-access)

- [**Rest API**](#rest-API)

  - [**Interface URL**](#Url)
  - [**Request**](#Request)
  - [**Signature**](#Signature)
  - [**Rest API**](#Rest-API-list)
  - [**Get symbols list**](#Get-symbols-list)
  - [**Get all tickers**](#Get-all-tickers)
  - [**Get recent fills**](#Get-latest-price-of-the-specified-symbol)
  - [**Obtain the current market of the designated symbol**](#Get-ticker)
  - [**Kline data**](#Kline-data)
  - [**Obtain the depth of order book**](#Get-depth-of-the-specified-symbol)
  - [**User balances**](#User-balances)
  - [**Order list**](#Order-list)
  - [**Order details**](#Order-details)
  - [**Creating order**](#Creating-order)
  - [**Cancel an order**](#Cancel-an-order)

- [**Websocket API**](#websocket-API)

  - [**Url**](#URL(socket))
  - [**Request**](#request(socket))
  - [**Top 5**](#Top-5:)
  - [**Top 20**](#Top-20:)

## Getting started

**Welcome to use the developer documentation. BitDATA provides an easy-to-use API interface through which you can obtain market data, conduct transactions and manage orders through API.**

### Create API Key

Email to bd@bitdata.pro to explain the purpose for key application 
> **Please do not leak Secret Key information to avoid asset losses. It is recommended that users bind IP addresses for API.**

### Description on the way of calling access

**BitDATA provides two kinds of calling access, which can be called by users according to their usage scenarios and preferences.**

- REST API

  Provide functions of market inquiry, balance inquiry, crypto exchange and order management. Users are advised to use REST API to query account balance, crypto exchange and order management.

- Websocket API

  Provide market data, trading depth and real-time trade information. Users are advised to use Websocket API to obtain market data.

<br>

## Rest API

###  URL

- [**https://openAPI.bitdata.com.cn**](https://openAPI.bitdata.com.cn) 

### Request

#### Introduce

All requests are based on Https protocol, and the content-type in the request header information needs to be uniformly formatted as the form:

- **content-type:application/x-www-form-urlencoded**

#### Code

code 200 succeed  The rest is failure

### Signature

#### Signature instructions

API requests are likely to be tampered  during network transmission. To ensure that the request has not been changed, all interfaces must use your secret_key for signature authentication so as to verify whether the parameter or parameter value has changed during the transmission.
#### Steps for signature

get parameter participates in signature. post parameter does not participate in signature authentication than enter parameter.
- (1) Field needed to be filled: timestamp: current timestamp APIkey: user key.
- (2) sort the ASCII codes according to the key of the parameters.
- (3) stitching parameters such as a=1&b=2&c=3.
- (4) Call the HmacSHA256 hash function with the generated string and API private key as parameters to obtain the hash value.
- (5) finally, signature= (4) is added to the url. 

php example：
```php
<?php
 $data = ["APIkey" => "APIkey", "timestamp"  =>  now()];
 ksort($data);
 $query = http_build_query($params);
 $signature = hash_hmac('sha256', $query, $this->userInfo['secret_key']); 
?>
```



### Rest API list

API                                              |  explain
------------------------------------------------ |  ---------------
[GET /market/symbols](#Get-symbols-list) |  Get symbols list
[GET /market/list](#Get-all-tickers)       | Get all tickers
[GET /market/price-all](#Get-latest-price-of-the-specified-symbol)           | Get latest price of the specified symbol
[GET /market/info](#Get-ticker)            | Get ticker
[GET /market/line](#Kline-data)              | Kline data
[GET /market/dept](#Get-depth-of-the-specified-symbol)       | Get depth of the specified symbol
[GET /user/balance](#User-balances)           | User balances
[GET /order/list](#Order-List)              | Order list
[GET /order/detail](#Order-details)                | Order details
[POST /order/create](#Creating-order)           |  Creating order
[POST /order/cancel](#Cancel-an-order)           | Cancel an order

### Get symbols list

#### GET [/market/symbols]

#### Parameter name: empty

#### Return the sample:

```
{
    'status':200,
    'code':0,
    'message': "success",
    'data': {
        'list':[
            {
                'base_currency':"BTC",//base currency
                'quote_currency':"USDT",//quote currency
                'symbol': "BTC-USDT",//symbol
                'is_support_stop_limit': true,//support stop-limit or not?
                'price_precision': 1,//Price accuracy
                'amount_precision': 6,//The number of precision
                'min_order_amt': "0.001",//Min order amount 
                'min_order_value': "10.00",//Min order value
                'status': "online"//status
            },
            {
                'base_currency': "ETH",
                'quote_currency': "USDT",
                'symbol': "ETH-USDT",
                'is_support_stop_limit': true,
                'price_precision': 2,
                'amount_precision': 4,
                'min_order_amt': "0.001",
                'min_order_value': "10.00",
                'status': "online"
            },
        ]
    }
}
```

### Get all tickers

#### GET [/market/list]

#### Parameter name: empty


#### Return the sample:

```python
{
  "status": 200,
  "code": 0,
  "message": "success",
  "data": [{
    "buy": 111, //buy
    "sell": 22, //sell
    "symbol": "DDAM-USDT",
    "high": 0.01, // top price 
    "vol": 1468201, //Trading volume
    "last": 0.0087, // latest price
    "low": 0.0082, //floor price
    "change": -11.22,//Change
    "rose": 0.0087 //Change rate
  }]
}
```

### Get latest price of the specified symbol

#### GET [/market/price-all]

#### Parameter name: empty


#### Return the sample:

```python
{
    "status": 200,
    "code": 0,
    "message": "success",
    "data": {
        "BDT-USDT": "0.033358531887466646",
        "BTC-USDT": "11735.358977320047",
        "EOS-BTC": "3.1926928552906544",
        "ETH-USDT": "389.7322956286041",
        "ETH-BTC": "395.2097568531904",
        "XRP-BTC": "0.30127377345186107",
        "XRP-ETH": "0.30227162486732795",
        "XRP-USDT": "0.29770663595506375",
        "BCH-BTC": "301.29813279391743",
        "BCH-USDT": "296.4067069406598",
        "LTC-USDT": "57.71297590815923",
        "LTC-ETH": "58.63137862146134",
        "LTC-BTC": "58.48261262270383",
        "BNB-BTC": "22.49385089956827",
        "BNB-ETH": "22.25178292335382",
        "BNB-USDT": "22.22232915109006",
    }
}
```

### Get ticker

#### GET [/market/info]

#### Parameter name: empty

Parameter name   | Is it neccessry | Data type   | Description   | Value range
------ | ---- | ------ | --- | -----------------------------
symbol | true | string | Symbol | ddam-usdt

#### Return the sample:

```python
{
	"status": 200,
	"code": 0,
	"message": "success",
	"data":[{
			"OKB-BTC": 5.760656 //Coin-Last Price
		}]

}
```


### Kline data

#### GET [/market/line]

####  Parameter name: 

Parameter name   | Is it neccessry | Data type    | Description                        | Value range
------ | ---- | ------ | ------------------------- | -----------------------------
symbol | true | string | Symbol                       | ddam-usdt
period | true | number | K-line cycle, minute as unit, 1 is 1 minute, 1 day is 1,440 |  1m 15m  1h  1d   market span 


#### Return the sample:

```python
{
  "status": 200,
  "code": 0,
  "message": "success",
  "data": {
    "fields": [
      "time", //time
      "open", //Opening Price
      "close",//Closing Price
      "low", //floor price
      "high",//top price 
      "volume"//Volume
    ],
    "values": [
        [
            1595977200,
            "0.032251",
            "0.032273",
            "0.03222",
            "0.032273",
            "39102.00"
        ],
        [
            1595980800,
            "0.032257",
            "0.032235",
            "0.032224",
            "0.032282",
            "299901.00"
        ],
        [
            1595984400,
            "0.032244",
            "0.032252",
            "0.03216",
            "0.0323",
            "328776.00"
        ],
    ]
  }
}
```

### Get depth of the specified symbol

#### GET [/market/dept]

#### Parameter name:

Parameter name   | Is it neccessry | Data type   | Description                      | Value range
------ | ---- | ------ | ----------------------- | -----------------------------
symbol | true | string | symbol                     | 	btc-usdt
type   | true | string | depth | 5 and 10 are supported,  default is 5 when unfilled


#### Return the sample:

```python
{
    'status':200,
    'code':0,
    'message': "success",
    'data': {
            'bids':[//sell order
                [
                "11154.48",//price
                "2.703338"//count
                ],
                [
                "11154.43",
                "0.425628"
                ],
                [
                "11154.24",
                "0.008848"
                ],
                [
                "11154.11",
                "0.014589"
                ],
                [
                "11153.96",
                "0.156964"
                ]
            ],
            'asks':[//buy order
                [
                "11151.9",//price    
                "1.73709396"//count
                ],
                [
                "11152.4",
                "0.5"
                ],
                [
                "11152.6",
                "0.03018122"
                ],
                [
                "11152.94",
                "0.02176891343448454"
                ],
                [
                "11152.95",
                "0.12"
                ]
        ]
    }
}
```

### User balances

#### GET [/user/balance]


#### Return the sample:

```python
{
    "status": 200,
    "code": 0,
    "message": "success",
    "data": [
        {
            "coin": "BDT",    
            "freeze": "150",  
            "balance": "100005.09514946"  
        },
        {
            "coin": "ETH",
            "freeze": "0",
            "balance": "4.99505"
        }
    ],
}
```

### Order list

#### GET [/order/list]


#### Parameter name:

Parameter name     | Is it neccessry   | Data type   | Description         | Value range
-------- | ----- | ------ | ---------- | ----
pageSize | false | string | Page Size      |
page     | false | string | page         |
symbol   | true  | string | symbol         |
action   | true  |  int   |  type   | 1 is open order, 2 is all orders

#### Return the sample:

```python
{
  "status": 200,
  "code": 0,
  "message": "success",
  "data": {
      "list":[
          {
                "order_id": "201910241622591738949826",
                "amount": "0.001000000000000",
                "price": "7406.700000000000000",
                "vol": "0.000000000000000",
                "field_amount": "0.000000000000000",
                "avg_price": "0.000000000000000",
                "fee": "0.000000000000000",
                "side": "buy",
                "status": "cancelled",
                "created_at": "2019-10-24 08:23:00",
                "updated_at": "2020-08-03 07:51:41",
                "symbol": "BTC-USDT",
                "trigger_condition": "",
                "type": "limit",
                "finished_at": "2020-08-03 07:51:41"
          },
          {
                "order_id": "201910241622591738949826",
                "amount": "0.001000000000000",
                "price": "7406.700000000000000",
                "vol": "0.000000000000000",
                "field_amount": "0.000000000000000",
                "avg_price": "0.000000000000000",
                "fee": "0.000000000000000",
                "side": "buy",
                "status": "cancelled",
                "created_at": "2019-10-24 08:23:00",
                "updated_at": "2020-08-03 07:51:41",
                "symbol": "BTC-USDT",
                "trigger_condition": "",
                "type": "limit",
                "finished_at": "2020-08-03 07:51:41"
          }
       ],
    "pagination": {
        "page": 1,  //page
        "pageSize": 20,  //pageSize
        "totalRaws": 26,  //totalRaws
        "totalPage": 2    //totalPage
    }
  }
}
```

### Order details

#### GET [/order/detail]

#### Parameter name:

Parameter name     | Is it neccessry  | Data type   | Description        | Value range
-------- | ---- | ------ | ---------- | ----
order_id | true | string | order_id        |


#### Return the sample:

```python
{
  "status": 200,
  "code": 0,
  "message": "success",
  "data": {
    "order_id": "201910241622591738949826",
    "amount": "0.001000000000000",
    "price": "7406.700000000000000",
    "vol": "0.000000000000000",
    "field_amount": "0.000000000000000",
    "avg_price": "0.000000000000000",
    "fee": "0.000000000000000",
    "side": "buy",
    "status": "cancelled",
    "created_at": "2019-10-24 08:23:00",
    "updated_at": "2020-08-03 07:51:41",
    "symbol": "BTC-USDT",
    "trigger_condition": "",
    "type": "limit",
    "finished_at": "2020-08-03 07:51:41"
  }
}
```

### Creating order

#### POST [/order/create]


#### Parameter name:

Parameter name    | Is it neccessry   | Data type   | Description            | Value range
------- | ----- | ------ | -------------- | ------------------------------------
side    | true  | string | Side            | BUY/SELL
type    | true  | string | Order type           | trade type（limit,market,stop-limit）
amount  | true  | string | Purchase quantity ( value range: when type=market is order value )  | Trading volume (value range: when type=market is order value)
price   | false | string | Order price           | 
symbol  | true  | string | symbol             |
time    | true  | string | timestamp            |
sign    | true  | string | signature             |



#### Return the sample:

```python
{
    "status": 200,
    "code": 0,
    "message": "success",
    "data": {
        "order_id": "202008051739201020469932"
    }
}
```

### Cancel an order

#### POST [/order/cancel]

#### Parameter name:

Parameter name     | Is it neccessry  | Data type   | Description        | Value range
-------- | ---- | ------ | ---------- | ----
contents | true | string |  orders list |  42040201,421421,421421|BTC-USDT |  1 就是订单号逗号隔开，2是交易对
type     | true | int    |  | 1. cancel according to the order; 2. cancel according to the symbol

#### Return the sample:

```python
{
    "status": 200,
    "code": 0,
    "message": "success",
    "data": {}
}
```

## Websocket API

### URL(socket)

#### [ws://ws.bitdata.com.cn/v1]

### Request(socket)

- The client needs to add a header when connecting. X-Uuid: the unique id customized for the user. X-Unit: denominated coin, USD by default. Optional ["USD", "CNY"]

#### Heartbeat message

- When the user's Websocket client connects to the Websocket server, the server sends ping messages to the user periodically. When the client receives the heartbeat message, it should return the pong message.

> When the Websocket server sends the `ping` message several times in a row but does not receive any `pong` message return, the server will actively disconnect from the client

### Subscribe to market trade data

#### Return the sample:

```json
{
    "op":"sub",//sub，unsub
    "ch":"market.trade.detail",//Subscribe the channel 
    "scope":"3400:2392:6666",//Subscribe the symbol, format: "base currency id: quote currency id: 6666 (fixed parameter)"
    "data":""
}
```

#### Return the sample:

```json
{
    "ch":"market.trade.detail",//Subscribe the channel 
    "scope":"3400:2392:6666",//Subscribe the symbol
    "data":{
        "data":[
            {
                "amount":"8374",
                "ts":1608717969,//timestamp
                "price":"0.056061",//price
                "side":"buy"//Side 
            }
        ]
    }
}
```

### Top 5:

#### Return the sample:
```json
{
    "op":"sub",//sub，unsub
    "ch":"market.depth.step5",//Subscribe the channel
    "scope":"3400:2392:6666",//Subscribe the symbol, format: "base currency id: quote currency id: 6666 (fixed parameter)"
    "data":""
}
```
#### Return after data changed：

```json
{
    "ch":"market.depth.step5",//Subscribe the channel
    "scope":"3400:2392:6666",//Subscribe the symbol
    "data":{
        "bids":[//卖盘
            [
                "0.05597",//price
                "2591"//amount
            ],
            [
                "0.05592",
                "677"
            ],
            [
                "0.05585",
                "2729.7"
            ],
            [
                "0.055804",
                "147.8"
            ],
            [//sell 1
                "0.055759",
                "1222.3"
            ]
        ],
        "asks":[//Buy
            [//buy 1
                "0.05617",
                "2593"
            ],
            [
                "0.05624",
                "2095"
            ],
            [
                "0.05629",
                "3955.3"
            ],
            [
                "0.05633",
                "2323"
            ],
            [
                "0.05639",
                "4783"
            ]
        ]
    },
    "ms":1608718027447
}
```

### Top 20:

#### Request sent after successful connection:
```json
{
    "op":"sub",//sub，unsub
    "ch":"market.depth.step20",//Subscribe the channel
    "scope":"3400:2392:6666",//Subscribe the symbol, format: "base currency id: quote currency id: 6666 (fixed parameter)"
    "data":""
}
```
#### Return after data changed ：

```json
{
    "ch":"market.depth.step20",//Subscribe the channel
    "scope":"3400:2392:6666",//Subscribe the symbol
    "data":{
        "bids":[//sell
            [
                "0.05597",//price
                "2591"//amount
            ],
            [
                "0.05592",
                "677"
            ],
            [
                "0.05585",
                "2729.7"
            ],
            [
                "0.055804",
                "147.8"
            ],
            [//sell 1
                "0.055759",
                "1222.3"
            ]
        ],
        "asks":[//buy
            [//buy 1
                "0.05617",
                "2593"
            ],
            [
                "0.05624",
                "2095"
            ],
            [
                "0.05629",
                "3955.3"
            ],
            [
                "0.05633",
                "2323"
            ],
            [
                "0.05639",
                "4783"
            ]
        ]
    },
    "ms":1608718027447
}
```
