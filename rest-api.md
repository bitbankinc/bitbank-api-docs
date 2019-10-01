[日本語](rest-api_JP.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Private REST API for Bitbank (2019-09-27)](#private-rest-api-for-bitbank-2019-09-27)
  - [General API Information](#general-api-information)
  - [Authorization](#authorization)
  - [General endpoints](#general-endpoints)
    - [Assets](#assets)
    - [Order](#order)
      - [Fetch order information](#fetch-order-information)
      - [Create new order](#create-new-order)
      - [Cancel order](#cancel-order)
      - [Cancel multiple orders](#cancel-multiple-orders)
      - [Fetch multiple orders](#fetch-multiple-orders)
      - [Fetch active orders](#fetch-active-orders)
    - [Trade](#trade)
      - [Fetch trade history](#fetch-trade-history)
    - [Withdrawal](#withdrawal)
      - [Get withdrawal accounts](#get-withdrawal-accounts)
    - [New withdrawal request](#new-withdrawal-request)
    - [Status](#status)
      - [Get exchange status](#get-exchange-status)
    - [Settings](#settings)
      - [Get all pairs info](#get-all-pairs-info)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Private REST API for Bitbank (2019-09-27)

## General API Information

- The base endpoint is: **[https://api.bitbank.cc/v1](https://api.bitbank.cc/v1)**
- Any endpoint can return an ERROR; the error payload is as follows:

```json
{
  "success": 0,
  "data": {
    "code": 20003
  }
}
```

- Specific error codes and messages defined in another document [errors.md](errors.md).
- For `GET` endpoints, parameters must be sent as a `query string`.
- For `POST` endpoints, the parameters may be sent as a `request body` with content type `application/json`.
- Parameters may be sent in any order.

## Authorization

- Private REST API requires requests contain additional HTTP Authorization headers with the following information.
  - ACCESS-KEY : ACCESS-KEY and API secret can be generated in your access key page.
  - ACCESS-NONCE : ACCESS-NONCE should be an integer and increased every time a new request is issued (you can use unix timestamp as ACCESS-NONCE).
  - ACCESS-SIGNATURE : Hash the following string with `HMAC-SHA256`, using your API secret as hash key.
    - GET: [ACCESS-NONCE, request path, query parameters] concatenated (include `/v1` in request path).
    - POST: [ACCESS-NONCE, JSON string of request body] concatenated (include query parameters in request body).

## General endpoints

### Assets

```txt
GET /user/assets
```

return user's asset list

**Parameters:**
None

**Response:**

```json
{
  "success": 1,
  "data": {
    "assets": [
      {
        "asset": "string",
        "amount_precision": 0,
        "onhand_amount": "string",
        "locked_amount": "string",
        "free_amount": "string",
        "withdrawal_fee": "string"
      }
    ]
  }
}
```

### Order

#### Fetch order information

```txt
GET /user/spot/order
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | pair enum: `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`
order_id | number | YES | order id

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | order id
pair | string | pair enum: `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`
side | string | `buy` or `sell`
type | string | `limit` or `market`
start_amount | string | order qty when placed
remaining_amount | string | qty not executed
executed_amount| string | qty executed
price | string | order price
average_price | string | avg executed price
ordered_at | number | ordered at
status | string | status enum: `UNFILLED`, `PARTIALLY_FILLED`, `FULLY_FILLED`, `CANCELED_UNFILLED`, `CANCELED_PARTIALLY_FILLED`

response format:

```json
{
  "success": 1,
  "data": {
    "order_id": 0,
    "pair": "string",
    "side": "string",
    "type": "string",
    "start_amount": "string",
    "remaining_amount": "string",
    "executed_amount": "string",
    "price": "string",
    "average_price": "string",
    "ordered_at": 0,
    "status": "string"
  }
}
```

#### Create new order

```txt
POST /user/spot/order
```

**Parameters(requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | pair enum: `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`
amount | string | YES | amount
price | string | NO | price
side | string | YES | `buy` or `sell`
type | string | YES | `limit` or `market`

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | order id
pair | string | pair enum: `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`
side | string | `buy` or `sell`
type | string | `limit` or `market`
start_amount | string | order qty when placed
remaining_amount | string | qty not executed
executed_amount| string | qty executed
price | string | order price
average_price | string | avg executed price
ordered_at | number | ordered at
status | string | status enum: `UNFILLED`, `PARTIALLY_FILLED`, `FULLY_FILLED`, `CANCELED_UNFILLED`, `CANCELED_PARTIALLY_FILLED`

response format:

```json
{
  "success": 1,
  "data": {
    "order_id": 0,
    "pair": "string",
    "side": "string",
    "type": "string",
    "start_amount": "string",
    "remaining_amount": "string",
    "executed_amount": "string",
    "price": "string",
    "average_price": "string",
    "ordered_at": 0,
    "status": "string"
  }
}
```

#### Cancel order

```txt
POST /user/spot/cancel_order
```

**Parameters(requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | pair enum: `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`
order_id | number | YES | order id

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | order id
pair | string | pair enum: `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`
side | string | `buy` or `sell`
type | string | `limit` or `market`
start_amount | string | order qty when placed
remaining_amount | string | qty not executed
executed_amount| string | qty executed
price | string | order price
average_price | string | avg executed price
ordered_at | number | ordered at
canceled_at | number | canceled at
status | string | status enum: `UNFILLED`, `PARTIALLY_FILLED`, `FULLY_FILLED`, `CANCELED_UNFILLED`, `CANCELED_PARTIALLY_FILLED`

response format:

```json
{
  "success": 1,
  "data": {
    "order_id": 0,
    "pair": "string",
    "side": "string",
    "type": "string",
    "start_amount": "string",
    "remaining_amount": "string",
    "executed_amount": "string",
    "price": "string",
    "average_price": "string",
    "ordered_at": 0,
    "canceled_at": 0,
    "status": "string"
  }
}
```

#### Cancel multiple orders

```txt
POST /user/spot/cancel_orders
```

**Parameters(requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | pair enum: `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`
order_ids | number[] | YES | order ids

**Response:**

```json
{
  "success": 1,
  "data": {
    "orders": [
      {
        "order_id": 0,
        "pair": "string",
        "side": "string",
        "type": "string",
        "start_amount": "string",
        "remaining_amount": "string",
        "executed_amount": "string",
        "price": "string",
        "average_price": "string",
        "ordered_at": 0,
        "canceled_at": 0,
        "status": "string"
      }
    ]
  }
}
```

#### Fetch multiple orders

```txt
POST /user/spot/orders_info
```

*(it should be a POST method)*

**Parameters (requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | pair enum: `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`
order_ids | number[] | YES | order ids

response format:

```json
{
  "success": 1,
  "data": {
    "orders": [
      {
        "order_id": 0,
        "pair": "string",
        "side": "string",
        "type": "string",
        "start_amount": "string",
        "remaining_amount": "string",
        "executed_amount": "string",
        "price": "string",
        "average_price": "string",
        "ordered_at": 0,
        "canceled_at": 0,
        "status": "string"
      }
    ]
  }
}
```

#### Fetch active orders

```txt
GET /user/spot/active_orders
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | pair enum: `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`
count | number | YES | take limit
from_id | number | YES | take from order id
end_id | number | YES | take until order id
since | number | YES | since unix timestamp
end | number | YES | end unix timestamp

response format:

```json
{
  "success": 1,
  "data": {
    "orders": [
      {
        "order_id": 0,
        "pair": "string",
        "side": "string",
        "type": "string",
        "start_amount": "string",
        "remaining_amount": "string",
        "executed_amount": "string",
        "price": "string",
        "average_price": "string",
        "ordered_at": 0,
        "status": "string"
      }
    ]
  }
}
```

### Trade

#### Fetch trade history

```txt
GET /user/spot/trade_history
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | pair enum: `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`
count | number | YES |take limit
order_id | number | YES | order id
since | number | YES | since unix timestamp
end | number | YES | emd unix timestamp
order | string | NO | histories in order(order enum: `asc`or `desc`, default to `desc`)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
trade_id | number | trade id
pair | string | pair enum: `btc_jpy`, `xrp_jpy`, `ltc_btc`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`
order_id | number | order id
side | string | `buy` or `sell`
type | string | `limit` or `market`
amount | string | amount
price | string | order price
maker_taker | string | maker or taker
fee_amount_base | string | base asset fee amount
fee_amount_quote | string | quote asset fee amount
executed_at | number | order executed at unix timestamp

response format:

```json
{
  "success": 1,
  "data": {
    "trades": [
      {
        "trade_id": 0,
        "pair": "string",
        "order_id": 0,
        "side": "string",
        "type": "string",
        "amount": "string",
        "price": "string",
        "maker_taker": "string",
        "fee_amount_base": "string",
        "fee_amount_quote": "string",
        "executed_at": 0
      }
    ]
  }
}
```

### Withdrawal

#### Get withdrawal accounts

```txt
GET /user/withdrawal_account
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
asset | string | YES | asset enum: `btc`, `xrp`, `ltc`, `eth`, `mona`, `bcc`

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | withdrawal account uuid
label | string | withdrawal account label
address | string | withdrawal address

response format:

```json
{
  "success": 1,
  "data": {
    "accounts": [
      {
        "uuid": "string",
        "label": "string",
        "address": "string"
      }
    ]
  }
}
```

### New withdrawal request

```txt
POST /user/request_withdrawal
```

**Parameters (requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
asset | string | YES | enum: `btc`, `xrp`, `ltc`, `eth`, `mona`, `bcc`
uuid | string | YES | withdrawal account's uuid
amount | string | YES | withdrawal amount
otp_token | string | NO | provide if MFA is set up
sms_token | string | NO | provide if MFA is set up

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | withdrawal account uuid
asset | string | enum: `btc`, `xrp`, `ltc`, `eth`, `mona`, `bcc`
account_uuid | string | account uuid
amount | number | withdrawal amount
fee | number | withdrawal fee
label | string | withdrawal account label
address | string | withdrawal address
txid | string | withdrawal transaction id
status | string | withdrawal status enum: `CONFIRMING`, `EXAMINING`, `SENDING`,  `DONE`, `REJECTED`, `CANCELED`, `CONFIRM_TIMEOUT`
requested_at | number| requested at unix timestamp

response format:

```json
{
  "success": 1,
  "data": {
    "uuid": "string",
    "asset": "string",
    "amount": 0,
    "account_uuid": "string",
    "fee": 0,
    "status": "string",
    "label": "string",
    "txid": "string",
    "address": "string",
    "requested_at": 0
  }
}
```

### Status

#### Get exchange status

*Not require authentication.*

```txt
GET /spot/status
```

**Parameters:**
None

**Response:**

Name | Type | Description
------------ | ------------ | ------------
pair | string | pair
status | string | enum: `NORMAL`, `BUSY`,  `VERY_BUSY`
min_amount| string | minimum order amount (The busier the exchange is, the higher the min_amount will be)

response format:

```json
{
  "success": 1,
  "data": {
    "statuses": [
      {
        "pair": "string",
        "status": "string",
        "min_amount": "string"
      }
    ]
  }
}
```

### Settings

#### Get all pairs info

*Not require authentication.*

```txt
GET /spot/pairs
```

**Parameters:**
None

**Response:**

Name | Type | Description
------------ | ------------ | ------------
name | string | pair name
base_asset | string | base asset
quote_asset | string | quote asset
maker_fee_rate_base | string | maker fee (base asset)
taker_fee_rate_base| string |taker fee (base asset)
maker_fee_rate_quote| string |maker fee (quote asset)
taker_fee_rate_quote| string |taker fee (quote asset)
unit_amount| string | minimum order amount
limit_max_amount| string |max order amount
market_max_amount| string |market order max amount
market_allowance_rate| string |market order allowance rate
price_digits| number | price digits count
amount_digits| number | amount digits count
is_stop_buy| boolean | buy order suspended flag
is_stop_sell| boolean | sell order suspended flag

response format:

```json
{
  "success": 1,
  "data": {
    "paris": [
      {
        "name": "string",
        "base_asset": "string",
        "maker_fee_rate_base": "string",
        "taker_fee_rate_base": "string",
        "maker_fee_rate_quote": "string",
        "taker_fee_rate_quote": "string",
        "unit_amount": "string",
        "limit_max_amount": "string",
        "market_max_amount": "string",
        "market_allowance_rate": "string",
        "price_digits": 0,
        "amount_digits": 0,
        "is_stop_buy": true,
        "is_stop_sell": true
      }
    ]
  }
}
```
