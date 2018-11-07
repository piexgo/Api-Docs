# Web Socket Streams for Piexgo (2018-11-07)

# General WSS information

* The base endpoint is: **wss://api.piexgo.co/ws**

* All symbols for streams are **UPPERCASE**, such as BTC_USDT

  

# Detailed Stream information

## Trade Streams

The Trade Streams push newest trade information.

  

**Stream Name:** trade:\<symbol>

**Subscribe**
```json
{
    "op":"subscribe",
    "args":"trade:BTC_USDT"
}
```
**Unsubscribe**
```json
{
    "op":"unsubscribe",
    "args":"trade:BTC_USDT"
}
```

**Response Payload:**

```json
{
    "market":"BTC_USDT",
    "price":67832200000000,
    "quantity":24000000,
    "timestamp":1541569853272940
}
```

  

## Orderbook Streams

The Orderbook Streams push the depth book.

  

**Stream Name:** orderBook10:\<symbol>

**Subscribe**
```json
{
    "op":"subscribe",
    "args":"orderBook10:BTC_USDT"
}
```
**Unsubscribe**
```json
{
    "op":"unsubscribe",
    "args":"orderBook10:BTC_USDT"
}
```
**Response Payload:**
```json
{
    "err_code":0,
    "err_msg":"",
    "Data":{
        "asks":[
            {
                "price":"6783.2200000000",
                "qty":"0.24000000",
                "timestamp":1541569853167087
            },
            {
                "price":"7006.4200000000",
                "qty":"21.57000000",
                "timestamp":1541569853167087
            }
        ],
        "bids":[
            {
                "price":"6704.6000000000",
                "qty":"0.01000000",
                "timestamp":1541569853167087
            },
            {
                "price":"6688.6900000000",
                "qty":"0.70000000",
                "timestamp":1541569853167087
            }
        ]
    }
}
```

## TODO
1. Aggregate Trade Streams
2. Kline Streams
3. All Market Tickers Stream
4. Partial Book Depth Streams
