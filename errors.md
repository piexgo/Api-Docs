# Error codes for PIEXGO 

Errors consist of two parts: an error code and a message. Codes are universal, but messages can vary. Here is the error JSON payload:
```javascript
{
  "err_code": 10201,
  "msg": "Invalid symbol."
}
```

## 101xx - General Server or Network issues
#### -10101 10102 10103 10104 DISCONNECTED
* Internal error; unable to process your request. Please try again.

## 104xx - Apikey or signature is invalid
#### -10440 Incorrect ApiKey
* `Apikey` or `signature` was invalid

#### -10440 ApiKey has been expired
* `Apikey` was expired

#### -10440 ApiKey has been expired
* Paramster `timestamp` or `recv_window` was expired

## 103xx  - Request issues
#### -10300 Parameters Error
* A parameter was enpty

#### -10301 Price Error
* Parameter `price` less than or equal to 0

#### -10302 Volume Error
* Parameter `volume` less than or equal to 0.0001

#### -10303 Precision Error
* Parameters precision incorrect
* Please get from this interface `/api/v1/symbols` 

#### -10304 Order Type Error
* `side` only buy/sell
* `type` only LIMIT/MARKET/STOP_LIMIT

#### -10305 Insufficient Balance
* Insufficient balance

#### -10306 10307 10308 Insufficient transaction amount
* Insufficient transaction amount
* `price` or `volume` need more
