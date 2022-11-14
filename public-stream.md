[日本語](public-stream_JP.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Web Socket Streams for Bitbank (2022-11-10)](#web-socket-streams-for-bitbank-2022-11-10)
  - [General WSS information](#general-wss-information)
  - [General endpoints](#general-endpoints)
    - [Ticker](#ticker)
    - [Transactions](#transactions)
    - [Depth Diff](#depth-diff)
    - [Depth Whole](#depth-whole)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Web Socket Streams for Bitbank (2022-11-10)

## General WSS information

- The base endpoint is: **wss://stream.bitbank.cc**.
- [socket.io 4.x](https://socket.io/docs/v4/) (Engine.io protocol v4) is used under the hood, and the following code examples are demonstrated with [github.com/websockets/wscat](https://github.com/websockets/wscat)
- From 2022-07-26, [socket.io](https://socket.io/) is upgraded from [2.x](https://socket.io/docs/v2/) to [4.x](https://socket.io/docs/v4/)
- The specification of disconnection after 6 hours is abolished. Continuously, if an error occurs the connection will be closed. Please check the error messages.

## General endpoints

### Ticker

Ticker channel name is `ticker_{pair}`, available pairs are [pair list](pairs.md).

**Response:**

Name | Type | Description
------------ | ------------ | ------------
sell | string | lowest sell price
buy | string | highest buy price
high | string | highest price in last 24 hours
low | string | lowest price in last 24 hours
open | string | open price at 24 hours ago
last | string | last executed price
vol | string | volume
timestamp | number | ticked at unix timestamp (milliseconds)

**Sample code:**

<details>
<summary>wscat</summary>
<p>

```sh
$ wscat -c 'wss://stream.bitbank.cc/socket.io/?EIO=4&transport=websocket'

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

Transactions channel name is `transactions_{pair}`, available pairs are [pair list](pairs.md).

**Response:**

Name | Type | Description
------------ | ------------ | ------------
transaction_id | number | transaction id
side | string | enum: `buy` or `sell`
amount | string | amount
price | string | price
executed_at | number | executed at unix timestamp (milliseconds)

**Sample code:**

<details>
<summary>wscat</summary>
<p>

```sh
$ wscat -c 'wss://stream.bitbank.cc/socket.io/?EIO=4&transport=websocket'

connected (press CTRL+C to quit)
< 0{"sid":"PG3FbI0WrKIP7hKMABH_","upgrades":[],"pingInterval":25000,"pingTimeout":60000}
< 40
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

Depth Diff channel name is `depth_diff_{pair}`, available pairs are [pair list](pairs.md).

**Response:**

Name | Type | Description
------------ | ------------ | ------------
a | [string, string][] | [ask, amount][]
b | [string, string][] | [bid, amount][]
t | number | published at unix timestamp (milliseconds)


**Sample code:**

<details>
<summary>wscat</summary>
<p>

```sh
$ wscat -c 'wss://stream.bitbank.cc/socket.io/?EIO=4&transport=websocket'
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
                "t": 1568344204624
            }
        }
    }
]
```

### Depth Whole

Whole depth channel name is `depth_whole_{pair}`, available pairs are [pair list](pairs.md).

**Response:**

Name | Type | Description
------------ | ------------ | ------------
asks | [string, string][] | [ask, amount][]
bids | [string, string][] | [bid, amount][]
timestamp | number | published at timestamp


**Sample code:**

<details>
<summary>wscat</summary>
<p>

```sh
$ wscat -c 'wss://stream.bitbank.cc/socket.io/?EIO=4&transport=websocket'
connected (press CTRL+C to quit)
< 0{"sid":"PR6GLrwlEFjzHIPrABBM","upgrades":[],"pingInterval":25000,"pingTimeout":60000}
< 40
> 42["join-room","depth_whole_xrp_jpy"]
< 42["message",{"room_name":"depth_whole_xrp_jpy","message":{"data":{"asks":[["26.928","1000.0000"],["26.929","56586.5153"],["26.930","218.3431"],["26.931","3123.8845"],["26.933","1799.0000"],["26.934","377.9136"],["26.938","3411.1507"],["26.950","80.0000"],["26.955","80.0000"],["26.958","7434.5900"],["26.959","15000.0000"],["26.960","15000.0000"],["26.964","10837.6620"],["26.979","15000.0000"], ...

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
                "timestamp": 1568344476514
            }
        }
    }
]
```
