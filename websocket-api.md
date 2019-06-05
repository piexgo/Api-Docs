# Web Socket Streams for Piexgo (2018-11-07)

# General WSS information

* The base test-endpoint is: **wss://api.piexgo.com/ws**

* All symbols for streams are **lowercase**, such as btc_usdt

  

# Detailed Stream information

## Trade Streams

The Trade Streams push newest trade information.

  

**Stream Name:** dealinfo.\<symbol>

**Subscribe:**
```json
{
    "op":"subscribe",
    "topic":"dealinfo.btc_usdt"
}
```
**Unsubscribe:**
```json
{
    "op":"unsubscribe",
    "topic":"dealinfo.btc_usdt"
}
```

**Response Payload:**

```json
{
    "topic":"dealinfo",
    "symbol":"btc_usdt",
    "deals":[
        {
            "time":1553326491272,
            "direct":0,
            "price":"4003.9000000000",
            "vol":"0.01510000"
        },
        {
            "time":1553326491273,
            "direct":0,
            "price":"4003.9000000000",
            "vol":"0.23490000"
        }
    ]
}
```

  

## Depth Streams

The Depth Streams push the depth book. 

  

**Stream Name:** depth.\<symbol>

**Subscribe:**
```json
{
    "op":"subscribe",
    "topic":"depth.btc_usdt"
}
```
**Unsubscribe:**
```json
{
    "op":"unsubscribe",
    "topic":"depth.btc_usdt"
}
```
**Response Payload:**
```json
{
    "topic":"depth",
    "symbol":"btc_usdt",
    "buy_book":[
        {
            "index":0,
            "price":3990.67,
            "qty":0.21
        },
        {
            "index":1,
            "price":3990.56,
            "qty":0.2
        },
        {
            "index":2,
            "price":3990.52,
            "qty":0.24
        },
        {
            "index":3,
            "price":3990.49,
            "qty":0.43
        },
        {
            "index":4,
            "price":3990.48,
            "qty":1.02
        }
    ],
    "sell_book":[
        {
            "index":0,
            "price":3990.69,
            "qty":0.07
        },
        {
            "index":1,
            "price":3990.89,
            "qty":0.18
        },
        {
            "index":2,
            "price":3991.01,
            "qty":1.4
        },
        {
            "index":3,
            "price":3991.28,
            "qty":1.97
        },
        {
            "index":4,
            "price":3991.29,
            "qty":0.46
        }
    ],
    "change_id":36296
}
```

## Account Info Streams

The orders and balance change. 

  

**Stream Name:** pinfo

**Subscribe:**
```json
{
    "op":"subscribe",
    "topic":"pinfo"
}
```
**Unsubscribe:**
```json
{
    "op":"unsubscribe",
    "topic":"pinfo"
}
```
**Order Response Payload:**
*** order status [0:open; 2:finish; 4:cancel]
```json
{
    "topic":"pinfo",
    "type":"order",
    "data":{
        "order_price":"7831.1600000000",
        "price":"7828.08",
        "stop":"0.0000000000",
        "market_price":"0.0000000000",
        "volume":"1.00000000",
        "remain":"0.00000000", 
        "amount":"7828.07520000",
        "type":"buy",
        "timestamp":1559734776,
        "order_id":"1136203593297424385",
        "market":"BTC_USDT",
        "status":2, 
        "flag":0,
        "id":1559734776923657000
    }
}
```
**Balance Response Payload:**
```json

{
    "topic":"pinfo",
    "type":"balance",
    "data":{
        "token":"BTC",
        "type":"balance",
        "value":"10000991076.10471725",
        "id":10897125
    }
}
```

## TODO
1. Kline Streams
2. All Market Tickers Stream
