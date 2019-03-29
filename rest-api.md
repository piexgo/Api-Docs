# Public Rest API for Piexgo 

# General API Information
* The  base prod-endpoint is: **https://api.piexgo.com**
* The base test-endpoint is: **https://api.piexgo.co**
* All endpoints return either a JSON object or array.


* Any endpoint can return an ERROR; the error payload is as follows:
```javascript
{
  "code": 1017,
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
* TODO 



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
* Add the resulting value to the query header as a Signature parameter. 

### Example 2 (MARKET): As a query string
* **Sort the query string components by byte order.:** 
quantity=10&recv_window=5000&side=BUY&symbol=BTC_USDT&timestamp=1552356480000&type=MARKET
* **HMAC SHA512 signature:** Calculate the Signature:
The signature is calculated with the query string and secret key as inputs to a keyed hash function.
* Add the resulting value to the query header as a Signature parameter. 

### Example 3 (STOP_LIMIT): As a query string
* **Sort the query string components by byte order.:** price=6830&quantity=10&recv_window=5000&side=BUY&stop_price=6830&symbol=BTC_USDT&timestamp=1552356480000&type=STOP_LIMIT
* **HMAC SHA512 signature:** Calculate the Signature:
The signature is calculated with the query string and secret key as inputs to a keyed hash function.
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
```javascript


```


### 24H ticker statistics
```
GET /api/v1/tickers
```

**Parameters:**
NONE

**Response:**
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
            "volume": 1707,
            "circulation": 0,
            "total": 0,
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
        "volume": 1734,
        "circulation": 17452050,
        "total": 21000000,
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
price | DECIMAL | NO |
quantity | DECIMAL | YES |
side | ENUM | YES |
symbol | STRING | YES |
type | ENUM | YES |
stop_price | DECIMAL | NO |
recv_window | LONG | YES |
timestamp | LONG | YES |



Additional mandatory parameters based on `type`:

Type | Additional mandatory parameters
------------ | ------------
`LIMIT` |  `quantity`, `price`
`MARKET` | `quantity`
`STOP_LIMIT` | `quantity`, `price`, `stop_price`



**Response:**
```javascript

```

### Cancel order
```
POST /api/v1/cancelOrder  (HMAC signature)
```
Cancel an open order.


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
order_id | STRING | YES |
recv_window | LONG | YES |
timestamp | LONG | YES |

**Response:**
```javascript

```

### Current open orders 
```
POST /api/v1/openOrders  (HMAC signature)
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
page | STRING | YES |

* 50 records per page, page parameters starting from 0

**Response:**
```javascript
{
    "err_code":0,
    "err_msg":"OK",
    "total":10,
    "next_page":false,
    "list":[]
}
```

### All orders (USER_DATA)
```
POST /api/v1/orderHistory  (HMAC signature)
```
Get all order hisroty (filled or canceled).


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
page | STRING | YES |



**Response:**
```javascript

```

### Account information (USER_DATA)
```
POST /api/v1/account  (HMAC signature)
```
Get current account information.


**Parameters:**
NONE

**Response:**
```javascript
{
    "err_code":0,
    "err_msg":"ok",
    "data":[]
}
```



