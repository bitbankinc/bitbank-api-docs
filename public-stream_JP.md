[English](public-stream.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [リアルタイムデータ配信API (2023-11-17)](#%E3%83%AA%E3%82%A2%E3%83%AB%E3%82%BF%E3%82%A4%E3%83%A0%E3%83%87%E3%83%BC%E3%82%BF%E9%85%8D%E4%BF%A1api-2023-11-17)
  - [API 概要](#api-%E6%A6%82%E8%A6%81)
  - [WSチャンネル一覧](#ws%E3%83%81%E3%83%A3%E3%83%B3%E3%83%8D%E3%83%AB%E4%B8%80%E8%A6%A7)
    - [ティッカー](#%E3%83%86%E3%82%A3%E3%83%83%E3%82%AB%E3%83%BC)
    - [約定履歴](#%E7%B4%84%E5%AE%9A%E5%B1%A5%E6%AD%B4)
    - [板情報の差分配信](#%E6%9D%BF%E6%83%85%E5%A0%B1%E3%81%AE%E5%B7%AE%E5%88%86%E9%85%8D%E4%BF%A1)
    - [板情報](#%E6%9D%BF%E6%83%85%E5%A0%B1)
  - [板情報の処理方法](#%E6%9D%BF%E6%83%85%E5%A0%B1%E3%81%AE%E5%87%A6%E7%90%86%E6%96%B9%E6%B3%95)
  - [サーキットブレイク情報](#%E3%82%B5%E3%83%BC%E3%82%AD%E3%83%83%E3%83%88%E3%83%96%E3%83%AC%E3%82%A4%E3%82%AF%E6%83%85%E5%A0%B1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# リアルタイムデータ配信API (2023-11-17)

## API 概要

- エンドポイント URL: **wss://stream.bitbank.cc**.
- [socket.io 4.x](https://socket.io/docs/v4/)（Engine.io protocol v4）によって実装されています。下記のサンプルコードでは[github.com/websockets/wscat](https://github.com/websockets/wscat) を利用しています。
- 2022-07-26 より [socket.io](https://socket.io/) が [2.x](https://socket.io/docs/v2/) -> [4.x](https://socket.io/docs/v4/) にバージョンアップされました。
- stream.bitbank.ccの6時間後に自動切断される仕様は廃止されました。エラーが発生した場合は切断されますので、エラー内容をご確認ください。

## WSチャンネル一覧

### ティッカー

ティッカーのwsチャンネルの名前： `ticker_{pair}`。
通貨ペアの一覧: [ペア一覧](pairs.md)。

circuit_break_info.mode が `NONE` 以外の場合、sellとbuyが反転する場合があります。

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
$ wscat -c 'wss://stream.bitbank.cc/socket.io/?EIO=4&transport=websocket'

connected (press CTRL+C to quit)
< 0{"sid":"bDAf6vgk5xPau87WAA1u","upgrades":[],"pingInterval":25000,"pingTimeout":60000}
> 40
< 40{"sid":"lUkRb31kqoS9cLPNMc0W"}
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
通貨ペアの一覧: [ペア一覧](pairs.md)。

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
$ wscat -c 'wss://stream.bitbank.cc/socket.io/?EIO=4&transport=websocket'

connected (press CTRL+C to quit)
< 0{"sid":"PG3FbI0WrKIP7hKMABH_","upgrades":[],"pingInterval":25000,"pingTimeout":60000}
> 40
< 40{"sid":"lUkRb31kqoS9cLPNMc0W"}
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
通貨ペアの一覧: [ペア一覧](pairs.md)。

circuit_break_info.mode が `NONE` 以外の場合、aとbが反転する場合があります。

**Response:**

Name | Type | Description
------------ | ------------ | ------------
a | [string, string][] | [ask, amount][]
b | [string, string][] | [bid, amount][]
ao | string \| undefined | asksの最高値よりも高いasksの数量。数量の変動がない場合はメッセージに含まれません。
bu | string \| undefined | bidsの最安値よりも安いbidsの数量。数量の変動がない場合はメッセージに含まれません。
au | string \| undefined | bidsの最安値よりも安いasksの数量。数量の変動がない場合はメッセージに含まれません。
bo | string \| undefined | asksの最高値よりも高いbidsの数量。数量の変動がない場合はメッセージに含まれません。
t | number | 日時（UnixTimeのミリ秒）
s | string | シーケンスID、単調増加しますが連続しているとは限りません

`a` (asks)と `b` (bids)のamountは絶対数量です。またamountが0なものはその気配値が無くなったことを示します。

`s` は `depth_whole_{pair}` の `sequenceId` と共通のシーケンスIDです。

使い方については [板情報の処理方法](#%E6%9D%BF%E6%83%85%E5%A0%B1%E3%81%AE%E5%87%A6%E7%90%86%E6%96%B9%E6%B3%95) の節をご覧ください。


<a name="depth-diff-sample-code"></a>**サンプルコード:**

<details>
<summary>wscat</summary>
<p>

```sh
$ wscat -c 'wss://stream.bitbank.cc/socket.io/?EIO=4&transport=websocket'
connected (press CTRL+C to quit)
< 0{"sid":"9-zd3P1G-BNu_w37ABMX","upgrades":[],"pingInterval":25000,"pingTimeout":60000}
> 40
< 40{"sid":"lUkRb31kqoS9cLPNMc0W"}
> 42["join-room","depth_diff_xrp_jpy"]
< 42["message",{"room_name":"depth_diff_xrp_jpy","message":{"data":{"a":[],"b":[["26.758","20000.0000"],["26.212","0"]],"t":1570080269609,"s":"1234567890"}}}]
< 42["message",{"room_name":"depth_diff_xrp_jpy","message":{"data":{"a":[],"b":[["26.212","1000.0000"],["26.815","0"]],"t":1570080270100,"s":"1234567893"}}}]
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
                "ao": "1",
                "bu": "1",
                "t": 1568344204624,
                "s": "1234567890"
            }
        }
    }
]
```

### 板情報

板情報のwsチャンネルの名前： `depth_whole_{pair}`
通貨ペアの一覧: [ペア一覧](pairs.md)。

circuit_break_info.mode が `NONE` 以外の場合、asksとbidsが反転する場合があります。

**Response:**

Name | Type | Description
------------ | ------------ | ------------
asks | [string, string][] | [ask, amount][]
bids | [string, string][] | [bid, amount][]
asks_over | string | asksの最高値よりも高いasksの数量
bids_under | string | bidsの最安値よりも安いbidsの数量
asks_under | string | asksの最安値よりも安いasksの数量。通常モードの場合は `0`
bids_over | string | bidsの最高値よりも高いbidsの数量。通常モードの場合は `0`
timestamp | number | timestamp
sequenceId | string | シーケンスID、単調増加しますが連続しているとは限りません

`sequenceId` は `depth_diff_{pair}` の `s` と共通のシーケンスIDです。

使い方については [板情報の処理方法](#%E6%9D%BF%E6%83%85%E5%A0%B1%E3%81%AE%E5%87%A6%E7%90%86%E6%96%B9%E6%B3%95) の節をご覧ください。


<a name="depth-whole-sample-code"></a>**サンプルコード:**

<details>
<summary>wscat</summary>
<p>

```sh
$ wscat -c 'wss://stream.bitbank.cc/socket.io/?EIO=4&transport=websocket'
connected (press CTRL+C to quit)
< 0{"sid":"PR6GLrwlEFjzHIPrABBM","upgrades":[],"pingInterval":25000,"pingTimeout":60000}
> 40
< 40{"sid":"lUkRb31kqoS9cLPNMc0W"}
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
                "asks_over": "0.123",
                "bids_under": "0.123",
                "asks_under": "0",
                "bids_over": "0",
                "timestamp": 1568344476514,
                "sequenceId": "1234567890"
            }
        }
    }
]
```

### サーキットブレイク情報

[Public API] サーキットブレイク情報を取得。

**Response:**

Name | Type | Description
------------ | ------------ | ------------
mode | string | `NONE` または `CIRCUIT_BREAK` または `FULL_RANGE_CIRCUIT_BREAK` または `RESUMPTION` または `LISTING`
upper_trigger_price | string \| null | CB突入判定価格上限。上場中で基準価格が無い場合、CB中はnull
lower_trigger_price | string \| null | CB突入判定価格下限。上場中で基準価格が無い場合、CB中はnull
fee_type | string | `NORMAL` または `SELL_MAKER` または `BUY_MAKER` または `DYNAMIC`
estimated_itayose_price | string \| null | 見積価格。ザラ場または見積価格が無い場合はnull
estimated_itayose_amount | string \| null | 見積数量。ザラ場であればnull
itayose_upper_price | string \| null | 参照価格レンジ上限。ザラ場、無期限、上場準備時はnull
itayose_lower_price | string \| null | 参照価格レンジ下限。ザラ場、無期限、上場準備時はnull
reopen_timestamp | number \| null | サーキットブレーク終了予定時刻（UnixTimeのミリ秒）。ザラ場、無期限、再開準備でCB終了予定時刻がない場合はnull
timestamp | number | 日時（UnixTimeのミリ秒）

`mode` および `fee_type` の詳細は[サーキットブレーカー制度](https://bitbank.cc/docs/circuit-breaker-mode/)のページをご確認ください。

**サンプルコード:**

<details>
<summary>wscat</summary>
<p>

```sh
$ wscat -c 'wss://stream.bitbank.cc/socket.io/?EIO=4&transport=websocket'

connected (press CTRL+C to quit)
< 0{"sid":"PG3FbI0WrKIP7hKMABH_","upgrades":[],"pingInterval":25000,"pingTimeout":60000}
> 40
< 40{"sid":"lUkRb31kqoS9cLPNMc0W"}
> 42["join-room","circuit_break_info_xrp_jpy"]
< 42["message",{"room_name":"circuit_break_info_xrp_jpy","message":{"data":{"mode":"NONE","estimated_itayose_price":null,"estimated_itayose_amount":null,"itayose_upper_price":null,"itayose_lower_price":null,"upper_trigger_price":"1200000","lower_trigger_price":"800000","fee_type":"NORMAL","reopen_timestamp":null,"timestamp":1570080162855}}}]
< 42["message",{"room_name":"circuit_break_info_xrp_jpy","message":{"data":{"mode":"CIRCUIT_BREAK","estimated_itayose_price":"1000000","estimated_itayose_amount":null,"itayose_upper_price":"1300000","itayose_lower_price":"800000","upper_trigger_price":null,"lower_trigger_price":null,"fee_type":"SELL_MAKER","reopen_timestamp":1234573890000,"timestamp":1570080162856}}}]
...

```

</p>
</details>

**レスポンスのフォーマット:**

```json
[
    "message",
    {
        "room_name": "circuit_break_info_btc_jpy",
        "message": {
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
    }
]
```

## 板情報の処理方法

あるペアのリアルタイムな板情報は以下のように `depth_whole_{pair}` および `depth_diff_{pair}` roomのメッセージを処理することで得られます:

1. `depth_whole_{pair}` および `depth_diff_{pair}` について受信を開始します。方法については[板情報](#depth-whole-sample-code)および[板情報の差分配信のサンプルコード](#depth-diff-sample-code)をご覧ください。
1. `depth_diff_{pair}` メッセージたちを継続的にバッファ（記憶）します。
1. `depth_diff_{pair}` メッセージを受信したら、
    * その数量が「非0」なら、板のその気配値について追加または上書きします。
    * その数量が「0」なら、板からその気配値を取り除きます。
1.  `depth_whole_{pair}` メッセージを受信したら、
    * 板をその `data` で置き換え、
    * バッファしておいた `depth_diff_{pair}` たちを、
        * その `s` の昇順で、
        * その `s` が `depth_whole_{pair}` の `sequenceId` より大きいものについてのみ、
    * 適用します。

`depth_whole_{pair}` の処理方法について例示します。
例えば以下の順でメッセージを受信した場合:

> diff{s=3}, diff{s=5}, diff{s=6}, diff{s=8}, whole{sequenceId=5}

`whole{sequenceId=5}` で板を置き換えた後、 `diff{s=6}` と `diff{s=8}` をこの順で適用しなおします。 `diff{s=3}` と `diff{s=5}` は無視します。

シーケンスIDは（少なくともroomごとには）巻き戻ることはありませんので、
バッファした `depth_diff_{pair}` メッセージたちのうち、その `s` が最後の `depth_whole_{pair}` メッセージの `sequenceId` より小さいか等しいものについて破棄することでリソース使用量を削減できます。

**注意事項:**

* `depth_diff_{pair}` メッセージはその時々のbest bid/askより200エントリに対するものしか配信されません。
  このため、正確な板情報を得るには `depth_whole_{pair}` メッセージで定期的に置き換えし続ける必要があります。
  （さもなくば価格変動時に変動方向の価格を見過すことになります。）
* `depth_whole_{pair}` メッセージは `depth_diff_{pair}` メッセージより遅延することがあります。
  このため、 `depth_diff_{pair}` メッセージをバッファし `depth_whole_{pair}` メッセージ受信時に再適用する必要があります。
