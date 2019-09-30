[日本語](public-stream_JP.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Web Socket Streams for Bitbank (2019-09-27)](#web-socket-streams-for-bitbank-2019-09-27)
  - [General WSS information](#general-wss-information)
  - [General endpoints](#general-endpoints)
    - [Ticker](#ticker)
    - [Transactions](#transactions)
    - [Depth Diff](#depth-diff)
    - [Depth Whole](#depth-whole)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Web Socket Streams for Bitbank (2019-09-27)

## General WSS information

- The base endpoint is: **wss://stream.bitbank.cc**
- A single connection to **stream.bitbank.cc** is only valid for 24 hours; expect to be disconnected at the 24 hour mark

## General endpoints

### Ticker

Ticker channel name is `ticker_{pair}`, available pairs are `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`.

**Response:**

Name | Type | Description
------------ | ------------ | ------------
sell | string | lowest sell price
buy | string | highest buy price
high | string | highest price in last 24 hours
low | string | lowest price in last 24 hours
last | string | last executed price
vol | string | volume
timestamp | number | ticked at unix timestamp

response format:

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

### Transactions

Transactions channel name is `transactions_{pair}`, available pairs are `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`.

**Response:**

Name | Type | Description
------------ | ------------ | ------------
transaction_id | number | transaction id
side | string | enum: `buy` or `sell`
amount | string | amount
price | string | price
executed_at | number | executed at unix timestamp

response format:

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

Depth Diff channel name is `depth_diff_{pair}`, available pairs are `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`.

**Response:**

Name | Type | Description
------------ | ------------ | ------------
a | [string, string][] | [ask, amount][]
b | [string, string][] | [bid, amount][]
t | number | published at unix timestamp

response format:

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

Whole depth channel name is `depth_whole_{pair}`, available pairs are `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`.

**Response:**

Name | Type | Description
------------ | ------------ | ------------
asks | [string, string][] | [ask, amount][]
bids | [string, string][] | [bid, amount][]
timestamp | number | published at timestamp

response format:

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
