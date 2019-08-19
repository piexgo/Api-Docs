# Public Rest API for Piexgo 

# General API Information
* The  base prod-endpoint is: **https://api.piexgo.com**
* The base test-endpoint is: **https://api.piexgo.co**
* All endpoints return either a JSON object or array.


* Any endpoint can return an ERROR; the error payload is as follows:
```javascript
{
  "err_code": 10201,
  "msg": "Invalid symbol."
}
```


# Endpoint security type
* Each endpoint has a security type that determines the how you will
  interact with it.
* API-keys are passed into the Rest API via the `KEY` and `signature`
  header.
* API-keys and secret-keys **are case sensitive**.
* By default, API-keys can access all secure routes.




* `TRADE` and `USER_DATA` endpoints are `SIGNED` endpoints.

# SIGNED Endpoint security
* `SIGNED` endpoints require an additional parameter, `signature`, to be
  sent in the  `query string`.
* Endpoints use `HMAC SHA256` signatures. The `HMAC SHA256 signature` is a keyed `HMAC SHA256` operation.
  Use your `secretKey` as the key and `totalParams` as the value for the HMAC operation.
* The `signature` is **case sensitive**.


## Timing security
* A `SIGNED` endpoint also requires a parameter, `timestamp`, to be sent which should be the millisecond timestamp of when the request was created and sent.
* An additional parameter, `recv_window`, may be sent to specify the number of milliseconds after timestamp the request is valid for. If `recv_window` is not sent, it defaults to 5000.
* The logic is as follows:
```javascript
if (timestamp < (server_time + 1000) && (server_time - timestamp) <= recv_window) {
  // process request
} else {
  // reject request
}
```



## SIGNED Endpoint Examples for POST /api/v1/order
Here is a step-by-step example of how to send a vaild signed payload.

Key | Value
------------ | ------------
apiKey | 23db31c6-6fd7-4041-9388-81dea6a05086
secretKey | 7bea08b911022f8eece9dbd0b86facb86011c666


Parameter | Value
------------ | ------------
price | 6830
quantity | 10
side | BUY/SELL
symbol | BTC_USDT
type | LIMIT/MARKET/STOP_LIMIT
stop_price | 6830
recv_window | 5000
timestamp | 1552356480000




### Example 1 (LIMIT): As a query string
* **Sort the query string components by byte order.:** price=6830&quantity=10&recv_window=5000&side=BUY&symbol=BTC_USDT&timestamp=1552356480000&type=LIMIT
* **HMAC SHA512 signature:** Calculate the Signature:
The signature is calculated with the query string and secret key as inputs to a keyed hash function.

```
[linux]$ echo -n "price=6830&quantity=10&recv_window=5000&side=BUY&symbol=BTC_USDT&timestamp=1552356480000&type=LIMIT" | openssl dgst -sha512 -hmac "4ad49bd70fa3674800e10e37d3cfe4ce433a8b94"
(stdin)= fc3729658558e0ca42c12c46b94feb269cdf3d87c4a649896c457472b4e4ab4ec1640b0882d5de9a33ed282c33bc171c95f3315b95b578fd64f87bde4ce945cd
```

* Add the resulting value to the query header as a Signature parameter. 

### Example 2 (MARKET): As a query string
* **Sort the query string components by byte order.:** 
quantity=10&recv_window=5000&side=BUY&symbol=BTC_USDT&timestamp=1552356480000&type=MARKET
* **HMAC SHA512 signature:** Calculate the Signature:
The signature is calculated with the query string and secret key as inputs to a keyed hash function.

```
[linux]$ echo -n "quantity=10&recv_window=5000&side=BUY&symbol=BTC_USDT&timestamp=1552356480000&type=MARKET" | openssl dgst -sha512 -hmac "4ad49bd70fa3674800e10e37d3cfe4ce433a8b94"
(stdin)= 340b0fdc5adcaa3116d1bf2623bae1efb3baefb6d027cfc2d07df9e650f33fa2e3d4a2a1af2099556ab2487caad2efb184c459d97eda38e24659ffd9bdffe9de
```

* Add the resulting value to the query header as a Signature parameter. 

### Example 3 (STOP_LIMIT): As a query string
* **Sort the query string components by byte order.:** price=6830&quantity=10&recv_window=5000&side=BUY&stop_price=6830&symbol=BTC_USDT&timestamp=1552356480000&type=STOP_LIMIT
* **HMAC SHA512 signature:** Calculate the Signature:
The signature is calculated with the query string and secret key as inputs to a keyed hash function.

```
[linux]$ echo -n "price=6830&quantity=10&recv_window=5000&side=BUY&stop_price=6830&symbol=BTC_USDT&timestamp=1552356480000&type=STOP_LIMIT" | openssl dgst -sha512 -hmac "4ad49bd70fa3674800e10e37d3cfe4ce433a8b94"
(stdin)= d7fad2e98edf9dc49ec37537195c5a143a8f51615d74a3282391f6b7f861b152b83518d8e93fdd1ce63fc5936ab70ee336d9e89364c704e85204c040d20dbb41
```

* Add the resulting value to the query header as a Signature parameter. 





### Check server time
```
GET /api/v1/time
```
Get the current server time


**Parameters:**
NONE

**Response:**

```javascript
{
    "server_time":1552356480000
}
```



### Pairs information
```
GET /api/v1/symbols
```
Current exchange  symbol information


**Parameters:**
NONE

**Response:**

Name | Type |  Description
------------ | ------------ | ------------ 
symbol | STRING | Market
maker_fee | DECIMAL | Maker fee
taker_fee | DECIMAL | Taker fee
amount_precision | INT | Order quantity precision
price_precision | INT | Order price precison

```javascript
{
    "data":[
        {
            "symbol": "BTC_USDT",
            "maker_fee": 0.0005,
            "taker_fee": 0.0015,
            "amount_precision": 4,
            "price_precision": 2
        }
    ],
    "msg":"ok",
    "err_code": 0
}
```

## Market Data endpoints
### Order book
```
GET /api/v1/orderBook
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |


**Response:**

Name | Type |  Description
------------ | ------------ | ------------ 
index | INT | Order index
price | STRING | Order price
volume | STRING | Order quantity

```javascript
{
    "asks":[
        {
            "index": 0,
            "price": "4029.89",
            "volume": "1.5118"
        }
    ],
    "bids":[
        {
            "index": 0,
            "price": "4029.62",
            "volume": "0.7400"
        }
    ],
    "ts":1541684184,
    "msg":"ok",
    "err_code": 0
}
```

### Recent trades
```
GET /api/v1/trades
```
Get recent trades (up to last 500).

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |

**Response:**

Name | Type |  Description
------------ | ------------ | ------------ 
time | LONG |  Order deal time
direct | INT | 0-BUY, 1-SELL 
price | STRING | Order average deal price
vol | STRING | Order deal quantity 


```javascript
{
    "data":{
        "time": 1553821666904,
        "direct": 0,
        "price": "4029.7800000000",
        "vol": "0.48000000"
    },
    "msg":"ok",
    "err_code": 0
}

```


### 24H ticker statistics
```
GET /api/v1/tickers
```

**Parameters:**
NONE

**Response:**

Name | Type |  Description
------------ | ------------ | ------------ 
symbol | STRING | BTC_USDT
last_price | DECIMAL | Today last price
open_price | DECIMAL | Today open price
hign | DECIMAL | Today highest price 
low | DECIMAL | Today lowest price
change | DECIMAL | Ups and downs
amount_1d | DECIMAL | BTC deal quantity
volume_1d | DECIMAL | USDT deal quantity

```javascript
{
    "data":{
        "BTC":{
            "symbol": "ETH_BTC",
            "last_price": 0.034283,
            "open_price": 0.03431,
            "high": 0.03431,
            "low": 0.034166,
            "change": -0.0786942582337489,
            "amount_1d": 32,
            "volume_1d": 1704.82
        }
    },
    "msg":"ok",
    "err_code": 0
}
```


### Symbol ticker

```
GET /api/v1/ticker
```
Latest price for a symbol

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |



**Response:**

```javascript
{
    "ticker":{
        "symbol": "BTC_USDT",
        "last_price": 4029.88,
        "open_price": 4029.05,
        "high": 4029.88,
        "low": 4022.71,
        "change": 0.02060038967001966,
        "amount_1d": 6977606.47,
        "volume_1d": 1728.66
    },
    "msg":"ok",
    "err_code": 0
}
```


## Trade or Account endpoints
### New order  (TRADE)
```
POST /api/v1/order  (HMAC signature)
```
Send in a new order.

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
price | DECIMAL | NO | Order price
quantity | DECIMAL | YES | Order quantity
side | ENUM | YES | BUY/SELL
symbol | STRING | YES | Order market
type | ENUM | YES | LIMIT/MARKET/STOP_LIMIT
stop_price | DECIMAL | NO | STOP_LIMIT Order stop pirce
recv_window | LONG | YES | Expire times
timestamp | LONG | YES | Server time



Additional mandatory parameters based on `type`:

Type | Additional mandatory parameters
------------ | ------------
`LIMIT` |  `quantity`, `price`
`MARKET` | `quantity`
`STOP_LIMIT` | `quantity`, `price`, `stop_price`


**Response:**

```javascript
{
    "err_code": 0,
    "msg": "ok",
    "order_id": "1234567890"
}
```

### Cancel order  (TRADE)
```
POST /api/v1/cancelOrder  (HMAC signature)
```
Cancel an open order.


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES | Order market
order_id | STRING | YES | Order ID
recv_window | LONG | YES | Expire times
timestamp | LONG | YES | Server time

**Response:**

```javascript
{
    "err_code":0,
    "msg":"ok"
}
```

### Cancel orders  (TRADE)
```
POST /api/v1/cancelOrders  (HMAC signature)
```
Cancel multiple open orders.


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES | Order market
order_id | STRING | YES | Orders ID Separated by commas (Up to 50.)
recv_window | LONG | YES | Expire times
timestamp | LONG | YES | Server time

**Response:**

```javascript
{
    "err_code":0,
    "msg":"ok"
}
```

### Single order info (USER_DATA) 
```
POST /api/v1/orderInfo  (HMAC signature)
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES | Order market
order_id | STRING | YES | Order ID

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_price | STRING | Order price |
price | STRING | Order average deal price |
volume | STRING | Order quantity |
filled_volume | STRING | Order filled quantity |
fee | STRING | Order fee |
type | STRING | buy/sell |
timestamp | LONG | Order created time |
update_time | LONG | Order updated time |
order_id | STRING | Order ID |
market | STRING | Market |
status | INT | Order status |
flag | INT | 0-LIMIT,1-MARKET,2-STOP_LIMIT |

**Definitions**

Order status (status)

* 0 : Order is pending
* 2 : Order is done
* 4 : Order is cancelled (Partial deal)
* 5 : Order is refused
* -1 : STOP_LIMIT order is pending

```javascript
{
    "err_code":0,
    "msg":"OK",
    "data":{
        "order_price": "3330.4800000000",
        "price": "3330.4800000000",
        "volume": "2.00000000",
        "filled_volume": "2.00000000",
        "fee":"0.00029486",
        "type": "buy",
        "timestamp": 1553831017,
        "update_time": 1553848365,
        "order_id": "1111070125244588064",
        "market": "BTC_USDT",
        "status": 0,
        "flag": 0,
    }
}
```

### Current open orders (USER_DATA) 
```
POST /api/v1/openOrders  (HMAC signature)
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
page | STRING | YES | Starting from zero

* 50 records per page, page parameters starting from 0

**Response:**

Name | Type | Description
------------ | ------------ | ------------
stop | STRING | Order stop price |
remain | STRING | Order remain quantity
market_price | STRING | MARKET Order price
stop | STRING | STOP_LIMIT Order stop price

```javascript
{
    "err_code":0,
    "msg":"OK",
    "total":10,
    "next_page":false,
    "list":[
        {
            "order_price": "3330.4800000000",
            "stop": "0.0000000000",
            "price": "3330.4800000000",
            "market_price": "0.0000000000",
            "volume": "2.00000000",
            "remain": "2.00000000",
            "amount": "0.00000000",
            "type": "buy",
            "timestamp": 1553831017,
            "order_id": "1111070125244588064",
            "market": "BTC_USDT",
            "status": 0,
            "flag": 0
        }
    ]
}
```

### Transaction History orders (USER_DATA)
```
POST /api/v1/tradeOrderHistory  (HMAC signature)
```
Get transaction order hisroty.


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
page | STRING | YES | Starting from zero



**Response:**

```javascript
{
    "err_code":0,
    "msg":"OK",
    "total":10,
    "next_page":false,
    "list":[
        {
            "order_price": "3330.4800000000",
            "stop": "0.0000000000",
            "price": "3330.4800000000",
            "market_price": "0.0000000000",
            "volume": "2.00000000",
            "remain": "2.00000000",
            "amount": "0.00000000",
            "type": "buy",
            "timestamp": 1553831017,
            "order_id": "1111070125244588064",
            "market": "BTC_USDT",
            "status": 4,
            "flag": 0
        }
    ]
}
```

### Canceled History orders (USER_DATA)
```
POST /api/v1/canceledOrderHistory  (HMAC signature)
```
Get canceled order hisroty.


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
page | STRING | YES | Starting from zero



**Response:**

```javascript
{
    "err_code":0,
    "msg":"OK",
    "total":10,
    "next_page":false,
    "list":[
        {
            "order_price": "3330.4800000000",
            "stop": "0.0000000000",
            "price": "3330.4800000000",
            "market_price": "0.0000000000",
            "volume": "2.00000000",
            "remain": "2.00000000",
            "amount": "0.00000000",
            "type": "buy",
            "timestamp": 1553831017,
            "order_id": "1111070125244588064",
            "market": "BTC_USDT",
            "status": 4,
            "flag": 0
        }
    ]
}
```

### History orders (USER_DATA)
```
POST /api/v1/orderHistory  (HMAC signature)(To be removed) 
```
Get all order hisroty (filled or canceled).


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES | Order market
page | STRING | YES | Starting from zero



**Response:**

```javascript
{
    "err_code":0,
    "msg":"OK",
    "total":10,
    "next_page":false,
    "list":[
        {
            "order_price": "3330.4800000000",
            "stop": "0.0000000000",
            "price": "3330.4800000000",
            "market_price": "0.0000000000",
            "volume": "2.00000000",
            "remain": "2.00000000",
            "amount": "0.00000000",
            "type": "buy",
            "timestamp": 1553831017,
            "order_id": "1111070125244588064",
            "market": "BTC_USDT",
            "status": 4,
            "flag": 0
        }
    ]
}
```

### Account information (USER_DATA)
```
POST /api/v1/account  (HMAC signature)
```
Get current account information.


**Parameters:**
NONE

**Response:**

Name | Type | Description
------------ | ------------ | ------------
market | STRING | Symbol |
balance | STRING | Symbol balance |
locked | STRING | Symbol locked |
otc_balance | STRING | Symbol otc balance |
otc_locked | STRING | Symbol otc locked |

```javascript
{
    "err_code":0,
    "msg":"ok",
    "data":[
        {
            "market": "USDT",   
            "balance": "10048.28242756",
            "locked": "0.00000000",
            "otc_balance": "0.00000000",
            "otc_locked": "0.00000000",
        }
    ]
}
```



