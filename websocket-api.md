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

## TODO
1. Kline Streams
2. All Market Tickers Stream
