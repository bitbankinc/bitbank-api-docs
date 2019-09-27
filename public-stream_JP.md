[English](public-stream.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [リアルタイムデータ配信API (2019-09-27)](#%E3%83%AA%E3%82%A2%E3%83%AB%E3%82%BF%E3%82%A4%E3%83%A0%E3%83%87%E3%83%BC%E3%82%BF%E9%85%8D%E4%BF%A1api-2019-09-27)
  - [API 概要](#api-%E6%A6%82%E8%A6%81)
  - [WSチャンネル一覧](#ws%E3%83%81%E3%83%A3%E3%83%B3%E3%83%8D%E3%83%AB%E4%B8%80%E8%A6%A7)
    - [ティッカー](#%E3%83%86%E3%82%A3%E3%83%83%E3%82%AB%E3%83%BC)
    - [約定履歴](#%E7%B4%84%E5%AE%9A%E5%B1%A5%E6%AD%B4)
    - [板情報の差分配信](#%E6%9D%BF%E6%83%85%E5%A0%B1%E3%81%AE%E5%B7%AE%E5%88%86%E9%85%8D%E4%BF%A1)
    - [板情報](#%E6%9D%BF%E6%83%85%E5%A0%B1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# リアルタイムデータ配信API (2019-09-27)

## API 概要

- エンドポイント URL: **wss://stream.bitbank.com**.

## WSチャンネル一覧

### ティッカー

ティッカーのwsチャンネルの名前： `ticker_{pair}`。
通貨ペアの一覧: `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`。

**Response:**

Name | Type | Description
------------ | ------------ | ------------
sell | string |現在の売り注文の最安値
buy | string | 現在の買い注文の最高値
high | string | 過去24時間の最高値取引価格
low | string | 過去24時間の最安値取引価格
last | string | 最新取引価格
vol | string | 過去24時間の出来高
timestamp | number | 日時（UnixTimeのミリ秒）

レスポンスのフォーマット:

```json
[
    "message",
    {
        "room_name": "ticker_btc_jpy",
        "message": {
            "pid": 0,
            "data": {
                "last": "string",
                "timestamp": 0,
                "sell": "string",
                "vol": "string",
                "buy": "string",
                "high": "string",
                "low": "string"
            }
        }
    }
]
```

### 約定履歴

約定履歴のwsチャンネルの名前： `transactions_{pair}`。
通貨ペアの一覧: `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`。

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
[
    "message",
    {
        "room_name": "transactions_btc_jpy",
        "message": {
            "pid": 0,
            "data": {
                "transactions": [
                    {
                        "side": "string",
                        "executed_at": 0,
                        "amount": "string",
                        "price": "string",
                        "transaction_id": 0
                    }
                ]
            }
        }
    }
]
```

### 板情報の差分配信

板情報の差分配信のwsチャンネルの名前： `depth_diff_{pair}`。
通貨ペアの一覧: `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`。

**Response:**

Name | Type | Description
------------ | ------------ | ------------
a | [string, string][] | [ask, amount][]
b | [string, string][] | [bid, amount][]
t | number | 日時（UnixTimeのミリ秒）

レスポンスのフォーマット:

```json
[
    "message",
    {
        "room_name": "depth_diff_xrp_jpy",
        "message": {
            "data": {
                "b": [
                    [
                        "26.872",
                        "43.3989"
                    ],
                    [
                        "26.871",
                        "100.0000"
                    ],
                ],
                "a": [
                    [
                        "27.839",
                        "1634.3980"
                    ],
                    [
                        "28.450",
                        "0"
                    ]
                ],
                "t": 1568344204624
            }
        }
    }
]
```

### 板情報

板情報のwsチャンネルの名前： `depth_whole_{pair}`
通貨ペアの一覧: `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`。

**Response:**

Name | Type | Description
------------ | ------------ | ------------
asks | [string, string][] | [ask, amount][]
bids | [string, string][] | [bid, amount][]
timestamp | number | timestamp

レスポンスのフォーマット:

```json
[
    "message",
    {
        "room_name": "depth_whole_xrp_jpy",
        "message": {
            "data": {
                "bids": [
                    [
                        "27.537",
                        "6211.6210"
                    ],
                    [
                        "27.523",
                        "875.3413"
                    ],
                ],
                "asks": [
                    [
                        "27.538",
                        "7233.6837"
                    ],
                    [
                        "27.540",
                        "19.4551"
                    ],
                ],
                "timestamp": 1568344476514
            }
        }
    }
]
```
