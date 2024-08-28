[日本語](public-stream_JP.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Web Socket Streams for Bitbank (2024-08-28)](#web-socket-streams-for-bitbank-2024-08-28)
  - [General WSS information](#general-wss-information)
  - [General endpoints](#general-endpoints)
    - [Ticker](#ticker)
    - [Transactions](#transactions)
    - [Depth Diff](#depth-diff)
    - [Depth Whole](#depth-whole)
    - [Circuit Break Info](#circuit-break-info)
  - [How to manage a local order book correctly](#how-to-manage-a-local-order-book-correctly)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Web Socket Streams for Bitbank (2024-08-28)

## General WSS information

- The base endpoint is: **wss://stream.bitbank.cc**.
- [socket.io 4.x](https://socket.io/docs/v4/) (Engine.io protocol v4) is used under the hood, and the following code examples are demonstrated with [github.com/websockets/wscat](https://github.com/websockets/wscat)
- From 2022-07-26, [socket.io](https://socket.io/) is upgraded from [2.x](https://socket.io/docs/v2/) to [4.x](https://socket.io/docs/v4/)
- The specification of disconnection after 6 hours is abolished. Continuously, if an error occurs the connection will be closed. Please check the error messages.

## General endpoints

### Ticker

Ticker channel name is `ticker_{pair}`, available pairs are written in [pair list](pairs.md).

Except for circuit_break_info.mode is `NONE`, sell and buy price possibly cross.

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

**Sample code:**

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

**Response format:**

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

### Transactions

Transactions channel name is `transactions_{pair}`, available pairs are written in [pair list](pairs.md).

**Response:**

Name | Type | Description
------------ | ------------ | ------------
transaction_id | number | transaction id
side | string | enum: `buy`, `sell`
price | string | price
amount | string | amount
executed_at | number | executed at unix timestamp (milliseconds)

**Sample code:**

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

**Response format:**

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

### Depth Diff

Depth Diff channel name is `depth_diff_{pair}`, available pairs are written in [pair list](pairs.md).

Except for circuit_break_info.mode is `NONE`, a and b price possibly cross.

**Response:**

Name | Type | Description
------------ | ------------ | ------------
a | [string, string][] | [ask, amount][]
b | [string, string][] | [bid, amount][]
ao | string \| undefined | optional. The quantity of asks over the highest price of asks orders. If there is no change in quantity, it will not be included in the message.
bu | string \| undefined | optional. The quantity of bids under the lowest price of bids orders. If there is no change in quantity, it will not be included in the message.
au | string \| undefined | optional. The quantity of asks under the lowest price of bids orders. If there is no change in quantity, it will not be included in the message.
bo | string \| undefined | optional. The quantity of bids over the highest price of asks orders. If there is no change in quantity, it will not be included in the message.
am | string \| undefined | optional. The quantity of market sell orders. If there is no change in quantity, it will not be included in the message.
bm | string \| undefined | optional. The quantity of market buy orders. If there is no change in quantity, it will not be included in the message.
t | number | published at unix timestamp (milliseconds)
s | string | sequence id, increased monotonically but not always consecutive

The amount of `a` (asks) and `b` (bids) is absolute, and its 0 means its price level has gone.

The `s` (sequence id) is common with the `sequenceId` of `depth_whole_{pair}`.

For usage, see [How to manage a local order book correctly](#how-to-manage-a-local-order-book-correctly) section.

<a name="depth-diff-sample-code"></a>**Sample code:**

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

**Response format:**

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
                "am": "1",
                "bm": "1",
                "t": 1568344204624,
                "s": "1234567890"
            }
        }
    }
]
```

### Depth Whole

Whole depth channel name is `depth_whole_{pair}`, available pairs are written in [pair list](pairs.md).

Except for circuit_break_info.mode is `NONE`, asks and bids price possibly cross.

#### In circuit_break_info.mode is `NONE` or estimated price is null

- Asks and bids data is restricted to 200 entries each from best bid offer.
- Asks and bids price never cross.

#### In circuit_break_info.mode is not `NONE` and estimated price is not null

- Asks and bids data is restricted to 200 entries each around the estimated price. (Max 400 entries)
- Asks and bids price possibly cross.
- asks_under, bids_over possibly includes the quantity of orders which price is out of range.

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

The `sequenceId` is common with the `s` of `depth_diff_{pair}`.

For usage, see [How to manage a local order book correctly](#how-to-manage-a-local-order-book-correctly) section.

<a name="depth-whole-sample-code"></a>**Sample code:**

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
< 42["message",{"room_name":"depth_whole_xrp_jpy","message":{"data":{"asks":[["26.928","1000.0000"],["26.929","56586.5153"],["26.930","218.3431"],["26.931","3123.8845"],["26.933","1799.0000"],["26.934","377.9136"],["26.938","3411.1507"],["26.950","80.0000"],["26.955","80.0000"],["26.958","7434.5900"],["26.959","15000.0000"],["26.960","15000.0000"],["26.964","10837.6620"],["26.979","15000.0000"], ...]}}}]

```

</p>
</details>

**Response format:**

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
                "ask_market": "0",
                "bid_market": "0",
                "timestamp": 1568344476514,
                "sequenceId": "1234567890"
            }
        }
    }
]
```

### Circuit Break Info

Get circuit break informations.

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

**Sample code:**

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

**Response format:**

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

## How to manage a local order book correctly

You can manage a local (typically on-memory) order book of a pair, by using room `depth_whole_{pair}` and `depth_diff_{pair}` as follows:

1. Subscribe both `depth_whole_{pair}` and `depth_diff_{pair}`. See [the sample code of Depth Whole](#depth-whole-sample-code) and [Depth Diff](#depth-diff-sample-code) for usage.
1. Buffer the `depth_diff_{pair}` messages you receive continuously.
1. When you receive a `depth_diff_{pair}` message,
    * If its amount is NOT zero, add or overwrite the amount of that price level to your local order book.
    * If its amount IS zero, remove that price level from your local order book.
1. When you receive a `depth_whole_{pair}` message,
    * replace your local order book by its `data`,
    * and apply buffered `depth_diff_{pair}`s,
        * that in ascending `s` order,
        * only for `s` is greater than the `sequenceId` of `depth_whole_{pair}`.

Here is an example of processing `depth_whole_{pair}`.
If you receive following messages in this order:

> diff{s=3}, diff{s=5}, diff{s=6}, diff{s=8}, whole{sequenceId=5}

you should replace the local order book by `whole{sequenceId=5}`, then apply `diff{s=6}` then `diff{s=8}` and ignore about `diff{s=3}` and `diff{s=5}`.

The sequence id never rewinds (at least on each room),
so you can forget `depth_diff_{pair}` messages that its `s` is less or equal than latest `depth_whole_{pair}`'s `sequenceId,` to reduce resource usage.

**Caveats:**

* `depth_diff_{pair}` messages are sent for only ~200 price levels from best bid/ask at that time.
  For that reason, you must refresh your local order book with each `depth_whole_{pair}` message to get the correct order book.
  (or you'll miss some price levels on price change.)
* A `depth_whole_{pair}` message sometimes be sent delayed from `depth_diff_{pair}` messages.
  For that reason, you must buffer `depth_diff_{pair}` messages and apply them on receiving a `depth_whole_{pair}` message.
