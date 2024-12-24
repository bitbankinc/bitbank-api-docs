[日本語](public-api_JP.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Public REST API for Bitbank](#public-rest-api-for-bitbank)
  - [General API Information](#general-api-information)
  - [General endpoints](#general-endpoints)
    - [Ticker](#ticker)
    - [Tickers](#tickers)
    - [TickersJPY](#tickersjpy)
    - [Depth](#depth)
      - [In circuit_break_info.mode is `NONE` or estimated price is null](#in-circuit_break_infomode-is-none-or-estimated-price-is-null)
      - [In circuit_break_info.mode is not `NONE` and estimated price is not null](#in-circuit_break_infomode-is-not-none-and-estimated-price-is-not-null)
    - [Transactions](#transactions)
    - [Candlestick](#candlestick)
    - [Circuit Break Info](#circuit-break-info)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Public REST API for Bitbank

## General API Information

- The base endpoint is: **[https://public.bitbank.cc](https://public.bitbank.cc)**
- HTTP `4XX` return codes are used for malformed requests; the issue is on the sender's side.
- Any endpoint can return an ERROR; the error payload is as follows:

```json
{
  "success": 0,
  "data": {
    "code": 10000
  }
}
```

## General endpoints

### Ticker

Get Ticker information

Except for continuous trading mode, it may be the case that sell <= buy.

```txt
GET /{pair}/ticker
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | pair enum: [pair list](pairs.md)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
sell | string | the lowest price of sell orders
buy | string | the highest price of buy orders
high | string | the highest price in last 24 hours
low | string | the lowest price in last 24 hours
open | string | the open price at 24 hours ago
last | string | the latest price executed
vol | string | trading volume in last 24 hours
timestamp | number | ticked at unix timestamp (milliseconds)

**Response format:**

```json
{
  "success": 1,
  "data": {
    "sell": "string",
    "buy": "string",
    "high": "string",
    "low": "string",
    "open": "string",
    "last": "string",
    "vol": "string",
    "timestamp": 0
  }
}
```

### Tickers

Get All Tickers information

Except for continuous trading mode, it may be the case that sell <= buy.

```txt
GET /tickers
```

**Parameters:**

nothing

**Response:**

Name | Type | Description
------------ | ------------ | ------------
pair | string | pair enum: [pair list](pairs.md)
sell | string | the lowest price of sell orders
buy | string | the highest price of buy orders
high | string | the highest price in last 24 hours
low | string | the lowest price in last 24 hours
open | string | the open price at 24 hours ago
last | string | the latest price executed
vol | string | trading volume in last 24 hours
timestamp | number | ticked at unix timestamp (milliseconds)

**Response format:**

```json
{
  "success": 1,
  "data": [{
    "pair": "string",
    "sell": "string",
    "buy": "string",
    "high": "string",
    "low": "string",
    "open": "string",
    "last": "string",
    "vol": "string",
    "timestamp": 0
  }]
}
```

### TickersJPY

Get All JPY Pair Tickers information

Except for continuous trading mode, it may be the case that sell <= buy.

```txt
GET /tickers_jpy
```

**Parameters:**

nothing

**Response:**

Name | Type | Description
------------ | ------------ | ------------
pair | string | JPY pair enum: [pair list](pairs.md)
sell | string | the lowest price of sell orders
buy | string | the highest price of buy orders
high | string | the highest price in last 24 hours
low | string | the lowest price in last 24 hours
open | string | the open price at 24 hours ago
last | string | the latest price executed
vol | string | trading volume in last 24 hours
timestamp | number | ticked at unix timestamp (milliseconds)

**Response format:**

```json
{
  "success": 1,
  "data": [{
    "pair": "string",
    "sell": "string",
    "buy": "string",
    "high": "string",
    "low": "string",
    "open": "string",
    "last": "string",
    "vol": "string",
    "timestamp": 0
  }]
}
```

### Depth

Get Depth information.

#### In circuit_break_info.mode is `NONE` or estimated price is null

- Asks and bids data is restricted to 200 entries each from best bid offer.
- Asks and bids price never cross.

#### In circuit_break_info.mode is not `NONE` and estimated price is not null

- Asks and bids data is restricted to 200 entries each around the estimated price. (Max 400 entries)
- Asks and bids price possibly cross.
- asks_under, bids_over possibly includes the quantity of orders which price is out of range.

```txt
GET /{pair}/depth
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | pair enum: [pair list](pairs.md)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
asks | [string, string][] | array of [price, amount]
bids | [string, string][] | array of [price, amount]
asks_over | string | the quantity of asks over the highest price of asks orders.
bids_under | string | the quantity of bids under the lowest price of bids orders.
asks_under | string | the quantity of asks under the lowest price of asks orders. Usually "0" in `NORMAL` mode.
bids_over | string | the quantity of bids over the highest price of bids orders. Usually "0" in `NORMAL` mode.
ask_market | string | the quantity of market sell orders. Usually "0" in `NORMAL` mode.
bid_market | string | the quantity of market buy orders. Usually "0" in `NORMAL` mode.
timestamp | number | published at timestamp
sequenceId | number | sequence id, increased monotonically but not always consecutive

**Response format:**

```json
{
  "success": 1,
  "data": {
    "asks": [
      [
        "string",  "string"
      ]
    ],
    "bids": [
      [
        "string",  "string"
      ]
    ],
    "asks_over": "string",
    "bids_under": "string",
    "asks_under": "string",
    "bids_over": "string",
    "ask_market": "string",
    "bid_market": "string",
    "timestamp": 0,
    "sequenceId": "string"
  }
}
```

### Transactions

Get latest executed transactions. If you omit YYYYMMDD, you can get the latest 60.

```txt
GET /{pair}/transactions/{YYYYMMDD}
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | pair enum: [pair list](pairs.md)
YYYYMMDD | string | NO | date formatted as `YYYYMMDD`

**Response:**

Name | Type | Description
------------ | ------------ | ------------
transaction_id | number | transaction id
side | string | enum: `buy`, `sell`
price | string | price
amount | string | amount
executed_at | number | executed at unix timestamp (milliseconds)

**Response format:**

```json
{
  "success": 1,
  "data": {
    "transactions": [
      {
        "transaction_id": 0,
        "side": "string",
        "price": "string",
        "amount": "string",
        "executed_at": 0
      }
    ]
  }
}
```

### Candlestick

Get candlestick information.

```txt
GET /{pair}/candlestick/{candle-type}/{YYYY}
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | pair enum: [pair list](pairs.md)
candle-type | string | YES | candle type enum: `1min`, `5min`, `15min`, `30min`, `1hour`, `4hour`, `8hour`, `12hour`, `1day`, `1week`, `1month`
YYYY | string | YES | date formatted as `YYYY` or  `YYYYMMDD`

- YYYY Format depends on the candle-type:
  - `YYYYMMDD`: `1min`, `5min`, `15min`, `30min`, `1hour`
  - `YYYY`: `4hour`, `8hour`, `12hour`, `1day`, `1week`, `1month`

**Response:**

Name | Type | Description
------------ | ------------ | ------------
type | string | candle type enum: `1min`, `5min`, `15min`, `30min`, `1hour`, `4hour`, `8hour`, `12hour`, `1day`, `1week`, `1month`
ohlcv | [string, string, string, string, string, number][] | [open, high, low, close, volume, **unix timestamp (milliseconds)**]

**Response format:**

```json
{
  "success": 1,
  "data": {
    "candlestick": [
      {
        "type": "string",
        "ohlcv": [
          [
            "string",
            "string",
            "string",
            "string",
            "string",
            0
          ]
        ]
      }
    ]
  }
}
```

### Circuit Break Info

Get circuit break informations.

```txt
GET /{pair}/circuit_break_info
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | pair enum: [pair list](pairs.md)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
mode | string | enum: `NONE`, `CIRCUIT_BREAK`, `FULL_RANGE_CIRCUIT_BREAK`, `RESUMPTION`, `LISTING`
estimated_itayose_price | string \| null | estimated price. Null if mode is `NONE` or when there is no estimated price.
estimated_itayose_amount | string \| null | estimated amount. Null if mode is `NONE`.
itayose_upper_price | string \| null | itayose price range upper limit. Null if mode is in `NONE`, `FULL_RANGE_CIRCUIT_BREAK` or `LISTING`.
itayose_lower_price | string \| null | itayose price range lower limit. Null if mode is in `NONE`, `FULL_RANGE_CIRCUIT_BREAK` or `LISTING`.
upper_trigger_price | string \| null | upper trigger price. Null if mode is not `NONE`.
lower_trigger_price | string \| null | lower trigger price. Null if mode is not `NONE`.
fee_type | string | enum: `NORMAL`, `SELL_MAKER`, `BUY_MAKER`, `DYNAMIC`
reopen_timestamp | number \| null | reopen timestamp(milliseconds). Null if mode is `NONE`, or reopen timestamp is undetermined yet.
timestamp | number | ticked at unix timestamp (milliseconds)

For details on `mode` and `fee_type`, please check the [Circuit breaker system](https://bitbank.cc/docs/circuit-breaker-mode/) page.

**Response format:**

```json
{
  "success": 1,
  "data": {
    "mode": "string",
    "estimated_itayose_price": "string",
    "estimated_itayose_amount": "string",
    "itayose_upper_price": "string",
    "itayose_lower_price": "string",
    "upper_trigger_price": "string",
    "lower_trigger_price": "string",
    "fee_type": "string",
    "reopen_timestamp": 0,
    "timestamp": 0
  }
}
```
