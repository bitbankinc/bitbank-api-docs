[日本語](rest-api_JP.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Private REST API for Bitbank (2021-04-01)](#private-rest-api-for-bitbank-2021-04-01)
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

# Private REST API for Bitbank (2021-04-01)

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
    - GET: [ACCESS-NONCE, full request path with query parameters] concatenated (include `/v1` in request path).
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

Name | Type | Description
------------ | ------------ | ------------
asset | string  | asset enum: `jpy`, `btc`, `xrp`, `ltc`, `eth`, `mona`, `bcc`, `xlm`, `qtum`, `bat`
free_amount | string | free amount
amount_precision | number | amount precision
onhand_amount | string | onhand amount
locked_amount | string | locked amount
withdrawal_fee | string | withdrawal fee
stop_deposit | boolean | deposit status
stop_withdrawal | boolean | withdrawal status

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/assets" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/assets
```

</p>
</details>


**Response format:**

```json
{
  "success": 1,
  "data": {
    "assets": [
      {
        "asset": "string",
        "free_amount": "string",
        "amount_precision": 0,
        "onhand_amount": "string",
        "locked_amount": "string",
        "withdrawal_fee": "string",
        "stop_deposit": false,
        "stop_withdrawal": false,
      },
      {
        "asset": "jpy",
        "free_amount": "string",
        "amount_precision": 0,
        "onhand_amount": "string",
        "locked_amount": "string",
        "withdrawal_fee": {
            "under": "string",
            "over": "string",
            "threshold": "string"
        },
        "stop_deposit": false,
        "stop_withdrawal": false,
    },
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
pair | string | YES | pair enum: `btc_jpy`, `xrp_jpy`, `xrp_btc`, `ltc_jpy`, `ltc_btc`, `eth_jpy`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`, `xlm_jpy`, `xlm_btc`, `qtum_jpy`, `qtum_btc`, `bat_jpy`, `bat_btc`
order_id | number | YES | order id

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | order id
pair | string | pair enum: `btc_jpy`, `xrp_jpy`, `xrp_btc`, `ltc_jpy`, `ltc_btc`, `eth_jpy`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`, `xlm_jpy`, `xlm_btc`, `qtum_jpy`, `qtum_btc`, `bat_jpy`, `bat_btc`
side | string | `buy` or `sell`
type | string | `limit` or `market`
start_amount | string | order qty when placed
remaining_amount | string | qty not executed
executed_amount| string | qty executed
price | string | order price
post_only | boolean | whether Post Only or not (present only if type = `limit`)
average_price | string | avg executed price
ordered_at | number | ordered at unix timestamp (milliseconds)
expire_at | number or null | expiration time in unix timestamp (milliseconds)
status | string | status enum: `UNFILLED`, `PARTIALLY_FILLED`, `FULLY_FILLED`, `CANCELED_UNFILLED`, `CANCELED_PARTIALLY_FILLED`

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/spot/order?pair=btc_jpy&order_id=1" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/spot/order?pair=btc_jpy\&order_id=1
```

</p>
</details>


**Response format:**

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
    "post_only": false,
    "average_price": "string",
    "ordered_at": 0,
    "expire_at": 0,
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
pair | string | YES | pair enum: `btc_jpy`, `xrp_jpy`, `xrp_btc`, `ltc_jpy`, `ltc_btc`, `eth_jpy`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`, `xlm_jpy`, `xlm_btc`, `qtum_jpy`, `qtum_btc`, `bat_jpy`, `bat_btc`
amount | string | YES | amount
price | string | NO | price
side | string | YES | `buy` or `sell`
type | string | YES | `limit` or `market`
post_only | boolean | NO | Post Only (`true` can be specified only if type = `limit`. default `false`)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | order id
pair | string | pair enum: `btc_jpy`, `xrp_jpy`, `xrp_btc`, `ltc_jpy`, `ltc_btc`, `eth_jpy`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`, `xlm_jpy`, `xlm_btc`, `qtum_jpy`, `qtum_btc`, `bat_jpy`, `bat_btc`
side | string | `buy` or `sell`
type | string | `limit` or `market`
start_amount | string | order qty when placed
remaining_amount | string | qty not executed
executed_amount| string | qty executed
price | string | order price
post_only | boolean | whether Post Only or not (present only if type = `limit`)
average_price | string | avg executed price
ordered_at | number | ordered at unix timestamp (milliseconds)
expire_at | number or null | expiration time in unix timestamp (milliseconds)
status | string | status enum: `UNFILLED`, `PARTIALLY_FILLED`, `FULLY_FILLED`, `CANCELED_UNFILLED`, `CANCELED_PARTIALLY_FILLED`

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export REQUEST_BODY='{"pair": "xrp_jpy", "price": "20", "amount": "1","side": "buy", "type": "limit"}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' -H "Content-Type: application/json" -d ''"$REQUEST_BODY"'' https://api.bitbank.cc/v1/user/spot/order
```

</p>
</details>


**Response format:**

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
    "post_only": false,
    "average_price": "string",
    "ordered_at": 0,
    "expire_at": 0,
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
pair | string | YES | pair enum: `btc_jpy`, `xrp_jpy`, `xrp_btc`, `ltc_jpy`, `ltc_btc`, `eth_jpy`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`, `xlm_jpy`, `xlm_btc`, `qtum_jpy`, `qtum_btc`, `bat_jpy`, `bat_btc`
order_id | number | YES | order id

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | order id
pair | string | pair enum: `btc_jpy`, `xrp_jpy`, `xrp_btc`, `ltc_jpy`, `ltc_btc`, `eth_jpy`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`, `xlm_jpy`, `xlm_btc`, `qtum_jpy`, `qtum_btc`, `bat_jpy`, `bat_btc`
side | string | `buy` or `sell`
type | string | `limit` or `market`
start_amount | string | order qty when placed
remaining_amount | string | qty not executed
executed_amount| string | qty executed
price | string | order price
post_only | boolean | whether Post Only or not (present only if type = `limit`)
average_price | string | avg executed price
ordered_at | number | ordered at unix timestamp (milliseconds)
expire_at | number or null | expiration time in unix timestamp (milliseconds)
canceled_at | number | canceled at unix timestamp (milliseconds)
status | string | status enum: `UNFILLED`, `PARTIALLY_FILLED`, `FULLY_FILLED`, `CANCELED_UNFILLED`, `CANCELED_PARTIALLY_FILLED`

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export REQUEST_BODY='{"pair": "xrp_jpy", "order_id": 1}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' -H "Content-Type: application/json" -d ''"$REQUEST_BODY"'' https://api.bitbank.cc/v1/user/spot/cancel_order
```

</p>
</details>


**Response format:**

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
    "post_only": false,
    "average_price": "string",
    "ordered_at": 0,
    "expire_at": 0,
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
pair | string | YES | pair enum: `btc_jpy`, `xrp_jpy`, `xrp_btc`, `ltc_jpy`, `ltc_btc`, `eth_jpy`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`, `xlm_jpy`, `xlm_btc`, `qtum_jpy`, `qtum_btc`, `bat_jpy`, `bat_btc`
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
        "post_only": false,
        "average_price": "string",
        "ordered_at": 0,
        "expire_at": 0,
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
pair | string | YES | pair enum: `btc_jpy`, `xrp_jpy`, `xrp_btc`, `ltc_jpy`, `ltc_btc`, `eth_jpy`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`, `xlm_jpy`, `xlm_btc`, `qtum_jpy`, `qtum_btc`, `bat_jpy`, `bat_btc`
order_ids | number[] | YES | order ids

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export REQUEST_BODY='{"pair": "xrp_jpy", "order_ids": [1]}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' -H "Content-Type: application/json" -d ''"$REQUEST_BODY"'' https://api.bitbank.cc/v1/user/spot/orders_info
```

</p>
</details>


**Response format:**

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
        "post_only": false,
        "average_price": "string",
        "ordered_at": 0,
        "expire_at": 0,
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
pair | string | YES | pair enum: `btc_jpy`, `xrp_jpy`, `xrp_btc`, `ltc_jpy`, `ltc_btc`, `eth_jpy`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`, `xlm_jpy`, `xlm_btc`, `qtum_jpy`, `qtum_btc`, `bat_jpy`, `bat_btc`
count | number | NO | take limit
from_id | number | NO | take from order id
end_id | number | NO | take until order id
since | number | NO | since unix timestamp
end | number | NO | end unix timestamp

**Response:**

Name | Type | Description
------------ | ------------ | ------------
pair | string | pair enum: `btc_jpy`, `xrp_jpy`, `xrp_btc`, `ltc_jpy`, `ltc_btc`, `eth_jpy`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`, `xlm_jpy`, `xlm_btc`, `qtum_jpy`, `qtum_btc`, `bat_jpy`, `bat_btc`
side | string | `buy` or `sell`
type | string | `limit` or `market`
start_amount | string | order qty when placed
remaining_amount | string | qty not executed
executed_amount| string | qty executed
price | string | order price
post_only | boolean | whether Post Only or not (present only if type = `limit`)
average_price | string | avg executed price
ordered_at | number | ordered at unix timestamp (milliseconds)
expire_at | number or null | expiration time in unix timestamp (milliseconds)
status | string | status enum: `UNFILLED`, `PARTIALLY_FILLED`, `FULLY_FILLED`, `CANCELED_UNFILLED`, `CANCELED_PARTIALLY_FILLED`

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/spot/active_orders?pair=btc_jpy" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/spot/active_orders?pair=btc_jpy
```

</p>
</details>


**Response format:**

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
        "post_only": false,
        "average_price": "string",
        "ordered_at": 0,
        "expire_at": 0,
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
pair | string | YES | pair enum: `btc_jpy`, `xrp_jpy`, `xrp_btc`, `ltc_jpy`, `ltc_btc`, `eth_jpy`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`, `xlm_jpy`, `xlm_btc`, `qtum_jpy`, `qtum_btc`, `bat_jpy`, `bat_btc`
count | number | NO | take limit (up to 1000)
order_id | number | NO | order id
since | number | NO | since unix timestamp
end | number | NO | emd unix timestamp
order | string | NO | histories in order(order enum: `asc` or `desc`, default to `desc`)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
trade_id | number | trade id
pair | string | pair enum: `btc_jpy`, `xrp_jpy`, `xrp_btc`, `ltc_jpy`, `ltc_btc`, `eth_jpy`, `eth_btc`, `mona_jpy`, `mona_btc`, `bcc_jpy`, `bcc_btc`, `xlm_jpy`, `xlm_btc`, `qtum_jpy`, `qtum_btc`, `bat_jpy`, `bat_btc`
order_id | number | order id
side | string | `buy` or `sell`
type | string | `limit` or `market`
amount | string | amount
price | string | order price
maker_taker | string | maker or taker
fee_amount_base | string | base asset fee amount
fee_amount_quote | string | quote asset fee amount
executed_at | number | order executed at unix timestamp (milliseconds)

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/spot/trade_history?pair=btc_jpy" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/spot/trade_history?pair=btc_jpy
```

</p>
</details>


**Response format:**

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
asset | string | YES | asset enum: `btc`, `xrp`, `ltc`, `eth`, `mona`, `bcc`, `xlm`, `qtum`, `bat`

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | withdrawal account uuid
label | string | withdrawal account label
address | string | withdrawal address

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/withdrawal_account?asset=btc" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/withdrawal_account?asset=btc
```

</p>
</details>


**Response format:**

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
asset | string | YES | enum: `btc`, `xrp`, `ltc`, `eth`, `mona`, `bcc`, `xlm`, `qtum`, `bat`
uuid | string | YES | withdrawal account's uuid
amount | string | YES | withdrawal amount
otp_token | string | NO | provide if MFA is set up
sms_token | string | NO | provide if MFA is set up

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | withdrawal account uuid
asset | string | enum: `btc`, `xrp`, `ltc`, `eth`, `mona`, `bcc`, `xlm`, `qtum`, `bat`
account_uuid | string | account uuid
amount | number | withdrawal amount
fee | number | withdrawal fee
label | string | withdrawal account label
address | string | withdrawal address
txid | string | withdrawal transaction id
status | string | withdrawal status enum: `CONFIRMING`, `EXAMINING`, `SENDING`,  `DONE`, `REJECTED`, `CANCELED`, `CONFIRM_TIMEOUT`
requested_at | number| requested at unix timestamp (milliseconds)


**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export REQUEST_BODY='{"asset": "xrp", "uuid": "___your uuid___", amount: "1"}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' -H "Content-Type: application/json" -d ''"$REQUEST_BODY"'' https://api.bitbank.cc/v1/user/request_withdrawal
```

</p>
</details>


**Response format:**

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
status | string | enum: `NORMAL`, `BUSY`, `VERY_BUSY`, `HALT`
min_amount| string | minimum order amount (The busier the exchange is, the higher the min_amount will be)

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
curl https://api.bitbank.cc/v1/spot/status
```

</p>
</details>


**Response format:**

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
taker_fee_rate_base | string | taker fee (base asset)
maker_fee_rate_quote | string | maker fee (quote asset)
taker_fee_rate_quote | string | taker fee (quote asset)
unit_amount | string | minimum order amount
limit_max_amount | string | max order amount
market_max_amount | string | market order max amount
market_allowance_rate | string | market order allowance rate
price_digits | number | price digits count
amount_digits | number | amount digits count
is_enabled | boolean | pair enable flag
stop_order | boolean | order suspended flag
stop_order_and_cancel | boolean | order and cancel suspended flag

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
curl https://api.bitbank.cc/v1/spot/pairs
```

</p>
</details>


**Response format:**

```json
{
  "success": 1,
  "data": {
    "pairs": [
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
        "is_enabled": true,
        "stop_order": false,
        "stop_order_and_cancel": false
      }
    ]
  }
}
```
