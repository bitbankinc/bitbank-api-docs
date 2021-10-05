[English](public-stream.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [リアルタイムデータ配信API (2021-10-05)](#%E3%83%AA%E3%82%A2%E3%83%AB%E3%82%BF%E3%82%A4%E3%83%A0%E3%83%87%E3%83%BC%E3%82%BF%E9%85%8D%E4%BF%A1api-2021-10-05)
  - [API 概要](#api-%E6%A6%82%E8%A6%81)
  - [WSチャンネル一覧](#ws%E3%83%81%E3%83%A3%E3%83%B3%E3%83%8D%E3%83%AB%E4%B8%80%E8%A6%A7)
    - [ティッカー](#%E3%83%86%E3%82%A3%E3%83%83%E3%82%AB%E3%83%BC)
    - [約定履歴](#%E7%B4%84%E5%AE%9A%E5%B1%A5%E6%AD%B4)
    - [板情報の差分配信](#%E6%9D%BF%E6%83%85%E5%A0%B1%E3%81%AE%E5%B7%AE%E5%88%86%E9%85%8D%E4%BF%A1)
    - [板情報](#%E6%9D%BF%E6%83%85%E5%A0%B1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# リアルタイムデータ配信API (2021-10-05)

## API 概要

- エンドポイント URL: **wss://stream.bitbank.cc**. ([socket.io](https://socket.io/)によって実装されています。下記のサンプルコードでは[github.com/websockets/wscat](https://github.com/websockets/wscat) を利用しています。)

## WSチャンネル一覧

### ティッカー

ティッカーのwsチャンネルの名前： `ticker_{pair}`。
通貨ペアの一覧: `btc_jpy`, `xrp_jpy`, `xrp_btc`, `ltc_jpy`, `ltc_btc`, `eth_jpy`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`, `xlm_jpy`, `xlm_btc`, `qtum_jpy`, `qtum_btc`, `bat_jpy`, `bat_btc`, `omg_jpy`, `omg_btc`, `xym_jpy`, `xym_btc`。

**Response:**

Name | Type | Description
------------ | ------------ | ------------
sell | string |現在の売り注文の最安値
buy | string | 現在の買い注文の最高値
high | string | 過去24時間の最高値取引価格
low | string | 過去24時間の最安値取引価格
open | string | 24時間前の始値
last | string | 最新取引価格
vol | string | 過去24時間の出来高
timestamp | number | 日時（UnixTimeのミリ秒）


**サンプルコード:**

<details>
<summary>wscat</summary>
<p>

```sh
$ wscat -c 'wss://stream.bitbank.cc/socket.io/?EIO=3&transport=websocket'

connected (press CTRL+C to quit)
< 0{"sid":"bDAf6vgk5xPau87WAA1u","upgrades":[],"pingInterval":25000,"pingTimeout":60000}
< 40
> 42["join-room","ticker_btc_jpy"]
< 42["message",{"room_name":"ticker_btc_jpy","message":{"pid":851203833,"data":{"sell":"896490","buy":"896489","open":"896489","high":"905002","low":"881500","last":"896489","vol":"650.2026","timestamp":1570080042822}}}]
< 42["message",{"room_name":"ticker_btc_jpy","message":{"pid":851203952,"data":{"sell":"896490","buy":"896489","open":"896489","high":"905002","low":"881500","last":"896489","vol":"650.2226","timestamp":1570080053768}}}]
...

```

</p>
</details>


**レスポンスのフォーマット:**

```json
[
    "message",
    {
        "room_name": "ticker_btc_jpy",
        "message": {
            "pid": 0,
            "data": {
                "last": "string",
                "open": "string",
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
通貨ペアの一覧: `btc_jpy`, `xrp_jpy`, `xrp_btc`, `ltc_jpy`, `ltc_btc`, `eth_jpy`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`, `xlm_jpy`, `xlm_btc`, `qtum_jpy`, `qtum_btc`, `bat_jpy`, `bat_btc`, `omg_jpy`, `omg_btc`, `xym_jpy`, `xym_btc`。

**Response:**

Name | Type | Description
------------ | ------------ | ------------
transaction_id | number | 取引ID
side | string | `buy` または `sell`
price | string | 価格
amount | string | 数量
executed_at | number | 約定日時（UnixTimeのミリ秒）


**サンプルコード:**

<details>
<summary>wscat</summary>
<p>

```sh
$ wscat -c 'wss://stream.bitbank.cc/socket.io/?EIO=3&transport=websocket'

connected (press CTRL+C to quit)
< 0{"sid":"PG3FbI0WrKIP7hKMABH_","upgrades":[],"pingInterval":25000,"pingTimeout":60000}
< 40
> 42["join-room","transactions_xrp_jpy"]
< 42["message",{"room_name":"transactions_xrp_jpy","message":{"pid":851205254,"data":{"transactions":[{"transaction_id":34745047,"side":"sell","price":"26.930","amount":"4703.5671","executed_at":1570080162855},{"transaction_id":34745046,"side":"sell","price":"26.930","amount":"500.0000","executed_at":1570080162829},{"transaction_id":34745045,"side":"sell","price":"26.930","amount":"378.0000","executed_at":1570080162802},{"transaction_id":34745044,"side":"sell","price":"26.930","amount":"12.0000","executed_at":1570080162758},{"transaction_id":34745043,"side":"sell","price":"26.930","amount":"301.4874","executed_at":1570080162725}]}}}]
...

```

</p>
</details>

**レスポンスのフォーマット:**

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
通貨ペアの一覧: `btc_jpy`, `xrp_jpy`, `xrp_btc`, `ltc_jpy`, `ltc_btc`, `eth_jpy`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`, `xlm_jpy`, `xlm_btc`, `qtum_jpy`, `qtum_btc`, `bat_jpy`, `bat_btc`, `omg_jpy`, `omg_btc`, `xym_jpy`, `xym_btc`。

**Response:**

Name | Type | Description
------------ | ------------ | ------------
a | [string, string][] | [ask, amount][]
b | [string, string][] | [bid, amount][]
t | number | 日時（UnixTimeのミリ秒）

**サンプルコード:**

<details>
<summary>wscat</summary>
<p>

```sh
$ wscat -c 'wss://stream.bitbank.cc/socket.io/?EIO=3&transport=websocket'
connected (press CTRL+C to quit)
< 0{"sid":"9-zd3P1G-BNu_w37ABMX","upgrades":[],"pingInterval":25000,"pingTimeout":60000}
< 40
> 42["join-room","depth_diff_xrp_jpy"]
< 42["message",{"room_name":"depth_diff_xrp_jpy","message":{"data":{"a":[],"b":[["26.758","20000.0000"],["26.212","0"]],"t":1570080269609}}}]
< 42["message",{"room_name":"depth_diff_xrp_jpy","message":{"data":{"a":[],"b":[["26.212","1000.0000"],["26.815","0"]],"t":1570080270100}}}]
...

```

</p>
</details>


**レスポンスのフォーマット:**

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
通貨ペアの一覧: `btc_jpy`, `xrp_jpy`, `xrp_btc`, `ltc_jpy`, `ltc_btc`, `eth_jpy`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`, `xlm_jpy`, `xlm_btc`, `qtum_jpy`, `qtum_btc`, `bat_jpy`, `bat_btc`, `omg_jpy`, `omg_btc`, `xym_jpy`, `xym_btc`。

**Response:**

Name | Type | Description
------------ | ------------ | ------------
asks | [string, string][] | [ask, amount][]
bids | [string, string][] | [bid, amount][]
timestamp | number | timestamp


**サンプルコード:**

<details>
<summary>wscat</summary>
<p>

```sh
$ wscat -c 'wss://stream.bitbank.cc/socket.io/?EIO=3&transport=websocket'
connected (press CTRL+C to quit)
< 0{"sid":"PR6GLrwlEFjzHIPrABBM","upgrades":[],"pingInterval":25000,"pingTimeout":60000}
< 40
> 42["join-room","depth_whole_xrp_jpy"]
< 42["message",{"room_name":"depth_whole_xrp_jpy","message":{"data":{"asks":[["26.928","1000.0000"],["26.929","56586.5153"],["26.930","218.3431"],["26.931","3123.8845"],["26.933","1799.0000"],["26.934","377.9136"],["26.938","3411.1507"],["26.950","80.0000"],["26.955","80.0000"],["26.958","7434.5900"],["26.959","15000.0000"],["26.960","15000.0000"],["26.964","10837.6620"],["26.979","15000.0000"], ...

```

</p>
</details>

**レスポンスのフォーマット:**

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
