# bitdata Open API

[Chinese](./open-en.md) | [English]

- [**Getting started**](#Starer-guide)

  - [**Create API Key**](#Create-API-Key)
  - [**Interface call description**](#Description-on-the-way-of-calling-access)

- [**REST API**](#rest-api)

  - [**Interface URL**](#URL)
  - [**Request**](#Request)
  - [**signature**](#signature)
  - [**REST API**](#REST-API-LIST)
  - [**Get Symbols List**](#Get-Symbols-List)
  - [**Get All Tickers**](#Get-All-Tickers)
  - [**Get Recent Fills**](#Get-latest-price-of-the-specified-symbol)
  - [**获取指定币对当前行情**](#获取指定币对当前行情)
  - [**Kline Data**](#Kline-Data)
  - [**获取买卖盘深度**](#获取买卖盘深度)
  - [**User Balances**](#User-Balances)
  - [**Order List**](#Order-List)
  - [**Order Details**](#Order-Details)
  - [**Creating Order**](#Creating-Order)
  - [**Cancel an Order**](#Cancel-an-Order)

- [**Websocket API**](#websocket-api)

  - [**URL**](#URL(socket))
  - [**request**](#request(socket))
  - [**买卖5档**](#买卖5档:)
  - [**买卖20档**](#买卖20档:)

## Starer guide

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

## REST API

###  URL

- **[https://open.bitdata.com](https://open.bitdata.com) **

### Request

#### Introduce

All requests are based on Https protocol, and the content-type in the request header information needs to be uniformly formatted as the form:

- **content-type:application/x-www-form-urlencoded**

#### Code

code 200 succeed  The rest is failure

### signature

#### Signature instructions

API requests are likely to be tampered  during network transmission. To ensure that the request has not been changed, all interfaces must use your secret_key for signature authentication so as to verify whether the parameter or parameter value has changed during the transmission.
#### Steps for signature

get parameter participates in signature. post parameter does not participate in signature authentication than enter parameter.
- (1) Field needed to be filled: timestamp: current timestamp apikey: user key.
- (2) sort the ASCII codes according to the key of the parameters.
- (3) stitching parameters such as a=1&b=2&c=3.
- (4) Call the HmacSHA256 hash function with the generated string and API private key as parameters to obtain the hash value.
- (5) finally, signature= (4) is added to the url. 

php example：
```php
<?php
 $data = ["apikey" => "apikey", "timestamp"  =>  now()];
 ksort($data);
 $query = http_build_query($params);
 $signature = hash_hmac('sha256', $query, $this->userInfo['secret_key']); 
?>
```



### REST API LIST

API                                              |  explain
------------------------------------------------ |  ---------------
[GET /market/symbols](#Get-Symbols-List) |  Get Symbols List
[GET /market/list](#Get-All-Tickers)       | Get All Tickers
[GET /market/price-all](#Get-latest-price-of-the-specified-symbol)           | Get latest price of the specified symbol
[GET /market/info](#Get-Ticker)            | Get Ticker
[GET /market/line](#Kline-Data)              | Kline Data
[GET /market/dept](#Get-depth-of-the-specified-symbol)       | Get depth of the specified symbol
[GET /user/balance](#User-Balances)           | User Balances
[GET /order/list](#Order-List)              | Order List
[GET /order/detail](#Order-Details)                | Order Details
[POST /order/create](#Creating-Order)           |  Creating Order
[POST /order/cancel](#Cancel-an-Order)           | Cancel an Order

### Get Symbols List

#### GET [/market/symbols]

#### Parameter name: empty

#### return the sample:

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
                'is_support_stop_limit': true,//是否支持止盈止损
                'price_precision': 1,//Price accuracy
                'amount_precision': 6,//The number of precision
                'min_order_amt': "0.001",//最小挂单量
                'min_order_value': "10.00",//最小挂单额
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

### Get All Tickers

#### GET [/market/list]

#### Parameter name: empty


#### return the sample:

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
    "vol": 1468201, //成交量
    "last": 0.0087, // latest price
    "low": 0.0082, //floor price
    "change": -11.22,//涨跌幅
    "rose": 0.0087 //涨跌比
  }]
}
```

### Get latest price of the specified symbol

#### GET [/market/price-all]

#### Parameter name: empty


#### return the sample:

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

### Get Ticker

#### GET [/market/info]

#### Parameter name: empty

Parameter name   | Is it neccessry | Data type   | Description   | Value range
------ | ---- | ------ | --- | -----------------------------
symbol | true | string | Symbol | ddam-usdt

#### return the sample:

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


### Kline Data

#### GET [/market/line]

####  Parameter name: 

Parameter name   | Is it neccessry | Data type    | Description                        | Value range
------ | ---- | ------ | ------------------------- | -----------------------------
symbol | true | string | Symbol                       | ddam-usdt
period | true | number | K线周期 单位为分钟,1代表1分钟 1天为1440 | 1m 15m  1h  1d   获取多久得行情


#### return the sample:

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
type   | true | string | depth | 支持5和10   不填默认为5


#### return the sample:

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

### User Balances

#### GET [/user/balance]


#### return the sample:

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

### Order List

#### GET [/order/list]


#### Parameter name:

Parameter name     | Is it neccessry   | Data type   | Description         | Value range
-------- | ----- | ------ | ---------- | ----
pageSize | false | string | Page Size      |
page     | false | string | page         |
symbol   | true  | string | symbol         |
action   | true  |  int   |  type   | 1当前委托  2所有委托

#### return the sample:

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

### Order Details

#### GET [/order/detail]

#### Parameter name:

Parameter name     | Is it neccessry  | Data type   | Description        | Value range
-------- | ---- | ------ | ---------- | ----
order_id | true | string | 订单号        |


#### return the sample:

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

### Creating Order

#### POST [/order/create]


#### Parameter name:

Parameter name    | Is it neccessry   | Data type   | Description            | Value range
------- | ----- | ------ | -------------- | ------------------------------------
side    | true  | string | 买卖方向           | BUY/SELL
type    | true  | string | 挂单类型           | trade type（limit,market,stop-limit）
amount  | true  | string | 购买数量(多义, 复用字段) | 订单交易量（市价买单此字段为订单交易额）
price   | false | string | 委托单价           | 
symbol  | true  | string | symbol             |
time    | true  | string | timestamp            |
sign    | true  | string | signature             |



#### return the sample:

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

### Cancel an Order

#### POST [/order/cancel]

#### Parameter name:

Parameter name     | Is it neccessry  | Data type   | Description        | Value range
-------- | ---- | ------ | ---------- | ----
contents | true | string |  orders list |  42040201,421421,421421|BTC-USDT |  1 就是订单号逗号隔开，2是交易对
type     | true | int    |  | 1为根据订单 取消 2为根据交易对进行取消 

#### return the sample:

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

### request(socket)

- The client needs to add a header when connecting. X-Uuid: the unique id customized for the user. X-Unit: denominated coin, default on USD. Optional ["USD", "CNY"]

#### 心跳消息

- When the user's Websocket client connects to the Websocket server, the server sends ping messages to the user periodically. When the client receives the heartbeat message, it should return the pong message.

> When the Websocket server sends the `ping` message several times in a row but does not receive any `pong` message return, the server will actively disconnect from the client

### 订阅实时成交信息

#### return the sample:

```json
{
    "op":"sub",//sub为订阅，unsub为取消订阅
    "ch":"market.trade.detail",//Subscribe the channel 
    "scope":"3400:2392:6666",//Subscribe the symbol, format: "base currency id: quote currency id: 6666 (fixed parameter)"
    "data":""
}
```

#### return the sample:

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
                "side":"buy"//交易方向
            }
        ]
    }
}
```

### 买卖5档:

#### return the sample:
```json
{
    "op":"sub",//sub，unsub
    "ch":"market.depth.step5",//Subscribe the channel
    "scope":"3400:2392:6666",//Subscribe the symbol, format: "base currency id: quote currency id: 6666 (fixed parameter)"
    "data":""
}
```
#### 数据变化后返回：

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
        "asks":[//卖盘
            [//买1
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

### 买卖20档:

#### 连接成功后发送请求:
```json
{
    "op":"sub",//sub为订阅，unsub为取消订阅
    "ch":"market.depth.step20",//订阅频道
    "scope":"3400:2392:6666",//订阅交易对,格式："基础币id:计价币id:6666（固定参数）"
    "data":""
}
```
#### 数据变化后返回：

```json
{
    "ch":"market.depth.step20",//订阅频道
    "scope":"3400:2392:6666",//订阅交易对
    "data":{
        "bids":[//卖盘
            [
                "0.05597",//价格
                "2591"//数量
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
            [//卖1
                "0.055759",
                "1222.3"
            ]
        ],
        "asks":[//卖盘
            [//买1
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
