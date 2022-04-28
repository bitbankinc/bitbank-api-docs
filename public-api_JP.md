[English](public-api.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Public API一覧 (2022-04-26)](#public-api%E4%B8%80%E8%A6%A7-2022-04-26)
  - [API 概要](#api-%E6%A6%82%E8%A6%81)
  - [エンドポイント一覧](#%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88%E4%B8%80%E8%A6%A7)
    - [Ticker](#ticker)
    - [Tickers](#tickers)
    - [TickersJPY](#tickersjpy)
    - [Depth](#depth)
    - [Transactions](#transactions)
    - [Candlestick](#candlestick)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Public API一覧 (2022-04-26)

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

- `GET`エンドポイントの場合、パラメータをクエリーストリングとして送信してください。

## エンドポイント一覧

### Ticker

[Public API] ティッカー情報を取得。

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

レスポンスのフォーマット:

```json
{
  "success": 0,
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

レスポンスのフォーマット:

```json
{
  "success": 0,
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

レスポンスのフォーマット:

```json
{
  "success": 0,
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

レスポンスのフォーマット:

```json
{
  "success": 0,
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
    ]
  }
}
```

### Transactions

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

レスポンスのフォーマット:

```json
{
  "success": 0,
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
ohlcv | string[] | [始値, 高値, 安値, 終値, 出来高, **UnixTimeのミリ秒**]

レスポンスのフォーマット:

```json
{
  "success": 0,
  "data": {
    "candlestick": [
      {
        "type": "string",
        "ohlcv": [
          [
            "string"
          ]
        ]
      }
    ]
  }
}
```
