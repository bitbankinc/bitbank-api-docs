[English](public-api.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Public API一覧 (2024-08-28)](#public-api%E4%B8%80%E8%A6%A7-2024-08-28)
  - [API 概要](#api-%E6%A6%82%E8%A6%81)
  - [エンドポイント一覧](#%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88%E4%B8%80%E8%A6%A7)
    - [Ticker](#ticker)
    - [Tickers](#tickers)
    - [TickersJPY](#tickersjpy)
    - [Depth](#depth)
      - [circuit_break_info.modeが `NONE` もしくは 見積価格がNull の場合](#circuit_break_infomode%E3%81%8C-none-%E3%82%82%E3%81%97%E3%81%8F%E3%81%AF-%E8%A6%8B%E7%A9%8D%E4%BE%A1%E6%A0%BC%E3%81%8Cnull-%E3%81%AE%E5%A0%B4%E5%90%88)
      - [circuit_break_info.modeが `NONE` 以外 かつ 見積価格が存在する 場合](#circuit_break_infomode%E3%81%8C-none-%E4%BB%A5%E5%A4%96-%E3%81%8B%E3%81%A4-%E8%A6%8B%E7%A9%8D%E4%BE%A1%E6%A0%BC%E3%81%8C%E5%AD%98%E5%9C%A8%E3%81%99%E3%82%8B-%E5%A0%B4%E5%90%88)
    - [Transactions](#transactions)
    - [Candlestick](#candlestick)
    - [Circuit Break Info](#circuit-break-info)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Public API一覧 (2024-08-28)

## API 概要

- エンドポイント URL: **[https://public.bitbank.cc](https://public.bitbank.cc)**
- リクエストに不正がある場合、HTTPステータス `4XX` を返します。
- リクエストに不正がある場合、以下のようなエラーレスポンスを返します。

```json
{
  "success": 0,
  "data": {
    "code": 10000
  }
}
```

## エンドポイント一覧

### ティッカー

[Public API] ティッカー情報を取得。

circuit_break_info.mode が `NONE` 以外の場合、sellとbuyが反転する場合があります。

```txt
GET /{pair}/ticker
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
sell | string | 現在の売り注文の最安値
buy | string | 現在の買い注文の最高値
high | string | 過去24時間の最高値取引価格
low | string | 過去24時間の最安値取引価格
open | string | 24時間前の始値
last | string | 最新取引価格
vol | string | 過去24時間の出来高
timestamp | number | 日時（UnixTimeのミリ秒）

**レスポンスのフォーマット:**

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

[Public API] 全ペアのティッカー情報を取得。

circuit_break_info.mode が `NONE` 以外の場合、sellとbuyが反転する場合があります。

```txt
GET /tickers
```

**Parameters:**

なし

**Response:**

Name | Type | Description
------------ | ------------ | ------------
pair | string | 通貨ペア: [ペア一覧](pairs.md)
sell | string | 現在の売り注文の最安値
buy | string | 現在の買い注文の最高値
high | string | 過去24時間の最高値取引価格
low | string | 過去24時間の最安値取引価格
open | string | 24時間前の始値
last | string | 最新取引価格
vol | string | 過去24時間の出来高
timestamp | number | 日時（UnixTimeのミリ秒）

**レスポンスのフォーマット:**

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

[Public API] JPYペアのティッカー情報を取得。
circuit_break_info.mode が `NONE` 以外の場合、sellとbuyが反転する場合があります。

```txt
GET /tickers_jpy
```

**Parameters:**

なし

**Response:**

Name | Type | Description
------------ | ------------ | ------------
pair | string | 通貨ペア(JPYペアのみ): [ペア一覧](pairs.md)
sell | string | 現在の売り注文の最安値
buy | string | 現在の買い注文の最高値
high | string | 過去24時間の最高値取引価格
low | string | 過去24時間の最安値取引価格
open | string | 24時間前の始値
last | string | 最新取引価格
vol | string | 過去24時間の出来高
timestamp | number | 日時（UnixTimeのミリ秒）

**レスポンスのフォーマット:**

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

[Public API] 板情報を取得。

#### circuit_break_info.modeが `NONE` もしくは 見積価格がNull の場合

- asks, bidsで配信されるデータは、Best Bid Offerから200件ずつです。
- したがって、asks, bidsのBBO（Best Bid Offer）は必ず `最も安いAsk > 最も高いBid` となります。

#### circuit_break_info.modeが `NONE` 以外 かつ 見積価格が存在する 場合

- asks, bidsで配信されるデータは、見積価格から上下200件ずつです。（最大400件）
- したがって、通常時とは異なり、 `最も安いAsk < 最も高いBid` となる場合があります。
- また、配信データの価格範囲よりも安い売り注文は `asks_under` に、高い買い注文は `bids_over` に加算されます。

```txt
GET /{pair}/depth
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
asks | [string, string][] | 売り板 [価格, 数量]
bids | [string, string][] | 買い板 [価格, 数量]
asks_over | string | asksの最高値よりも高いasksの数量
bids_under | string | bidsの最安値よりも安いbidsの数量
asks_under | string | bidsの最安値よりも安いasksの数量。通常モードの場合は `0`
bids_over | string | asksの最高値よりも高いbidsの数量。通常モードの場合は `0`
ask_market | string | 成行売り数量。通常モードの場合は `0`
bid_market | string | 成行買い数量。通常モードの場合は `0`
timestamp | number | timestamp
sequenceId | number | シーケンスID、単調増加しますが連続しているとは限りません

**レスポンスのフォーマット:**

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

### 約定履歴

[Public API] 指定された日付の全約定履歴を取得。YYYYMMDDを省略した場合、最新60件が取得可能。

```txt
GET /{pair}/transactions/{YYYYMMDD}
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)
YYYYMMDD | string | NO | date formatted as `YYYYMMDD`

**Response:**

Name | Type | Description
------------ | ------------ | ------------
transaction_id | number | 取引ID
side | string | `buy` または `sell`
price | string | 価格
amount | string | 数量
executed_at | number | 約定日時（UnixTimeのミリ秒）

**レスポンスのフォーマット:**

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

[Public API] 指定された日付のロウソク足データを取得。

```txt
GET /{pair}/candlestick/{candle-type}/{YYYY}
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)
candle-type | string | YES | 以下の期間から指定: `1min`, `5min`, `15min`, `30min`, `1hour`, `4hour`, `8hour`, `12hour`, `1day`, `1week`, `1month`
YYYY | string | YES | 日付 `YYYYMMDD` 形式または `YYYY` を指定

- YYYY の指定は candle-type によって異なります:
  - `YYYYMMDD`: `1min`, `5min`, `15min`, `30min`, `1hour`
  - `YYYY`: `4hour`, `8hour`, `12hour`, `1day`, `1week`, `1month`

**Response:**

Name | Type | Description
------------ | ------------ | ------------
type | string | 以下の期間から指定: `1min`, `5min`, `15min`, `30min`, `1hour`, `4hour`, `8hour`, `12hour`, `1day`, `1week`, `1month`
ohlcv | [string, string, string, string, string, number][] | [始値, 高値, 安値, 終値, 出来高, **UnixTimeのミリ秒**]

**レスポンスのフォーマット:**

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

### サーキットブレイク情報

[Public API] サーキットブレイク情報を取得。

```txt
GET /{pair}/circuit_break_info
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
mode | string | `NONE` または `CIRCUIT_BREAK` または `FULL_RANGE_CIRCUIT_BREAK` または `RESUMPTION` または `LISTING`
estimated_itayose_price | string \| null | 見積価格。ザラ場または見積価格が無い場合はnull
estimated_itayose_amount | string \| null | 見積数量。ザラ場であればnull
itayose_upper_price | string \| null | 参照価格レンジ上限。ザラ場、無期限、上場準備時はnull
itayose_lower_price | string \| null | 参照価格レンジ下限。ザラ場、無期限、上場準備時はnull
upper_trigger_price | string \| null | CB突入判定価格上限。CB中はnull
lower_trigger_price | string \| null | CB突入判定価格下限。CB中はnull
fee_type | string | `NORMAL` または `SELL_MAKER` または `BUY_MAKER` または `DYNAMIC`
reopen_timestamp | number \| null | サーキットブレイク終了予定時刻（UnixTimeのミリ秒）。ザラ場、またはCB終了予定時刻がない場合はnull
timestamp | number | 日時（UnixTimeのミリ秒）

`mode` および `fee_type` の詳細は[サーキットブレーカー制度](https://bitbank.cc/docs/circuit-breaker-mode/)のページをご確認ください。

**レスポンスのフォーマット:**

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
