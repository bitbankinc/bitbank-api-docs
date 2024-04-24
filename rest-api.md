[日本語](rest-api_JP.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Private REST API for Bitbank (2024-04-23)](#private-rest-api-for-bitbank-2024-04-23)
  - [General API Information](#general-api-information)
  - [Authorization](#authorization)
  - [Rate limit](#rate-limit)
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
    - [Deposit](#deposit)
      - [Fetch deposit history](#fetch-deposit-history)
      - [Fetch unconfirmed deposits](#fetch-unconfirmed-deposits)
      - [Fetch deposit originators](#fetch-deposit-originators)
      - [Confirm deposits](#confirm-deposits)
      - [Confirm all deposits](#confirm-all-deposits)
    - [Withdrawal](#withdrawal)
      - [Get withdrawal accounts](#get-withdrawal-accounts)
      - [New withdrawal request](#new-withdrawal-request)
      - [Fetch withdrawal history](#fetch-withdrawal-history)
    - [Status](#status)
      - [Get exchange status](#get-exchange-status)
    - [Settings](#settings)
      - [Get all pairs info](#get-all-pairs-info)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Private REST API for Bitbank (2024-04-23)

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

## Rate limit

- We limits REST API calls per user, per UPDATE or QUERY, per second.
  - UPDATE is order, cancel and withdrawal request.
  - QUERY is others.
- We also limits REST API calls system wide, to avoid overload of our matching engine.
  - This limit is high enough for usual case.
- We return HTTP 429 when you reached a limit. Please reduce API calls when you get 429 often.
- We usually limits QUERY requests to 10 calls per second and UPDATE requests to 6 calls per second.
  - We can raise your rate limits depending on your usage.
  - Please contact `onboarding@bitcoinbank.co.jp` from your bitbank account's email address to raise limits.

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
asset | string  | asset enum: [asset list](assets.md)
free_amount | string | free amount
amount_precision | number | amount precision
onhand_amount | string | onhand amount
locked_amount | string | locked amount
withdrawal_fee | { min: string, max: string } or { under: string, over: string, threshold:string } for `jpy` | withdrawal fee
stop_deposit | boolean | deposit status（All networks: stop_deposit = `true`）
stop_withdrawal | boolean | withdrawal status（All networks: stop_withdrawal = `true`）
network_list | { asset: string, network: string, stop_deposit: boolean, stop_withdrawal: boolean, withdrawal_fee: string } or undefined for `jpy` | network list

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
        "withdrawal_fee": {
            "min": "string",
            "max": "string"
        },
        "stop_deposit": false,
        "stop_withdrawal": false,
        "network_list": [
            {
                "asset": "string",
                "network": "string",
                "stop_deposit": false,
                "stop_withdrawal": false,
                "withdrawal_fee": "string"
            }
        ]
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
pair | string | YES | pair enum: [pair list](pairs.md)
order_id | number | YES | order id

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | order id
pair | string | pair enum: [pair list](pairs.md)
side | string | `buy` or `sell`
type | string | `limit` or `market` or `stop` or `stop_limit`
start_amount | string | order qty when placed
remaining_amount | string | qty not executed
executed_amount| string | qty executed
price | string \| undefined | order price (present only if type = `limit` or `stop_limit`)
post_only | boolean \| undefined | whether Post Only or not (present only if type = `limit`)
average_price | string | avg executed price
ordered_at | number | ordered at unix timestamp (milliseconds)
expire_at | number \| null | expiration time in unix timestamp (milliseconds)
triggered_at | number \| undefined | triggered at unix timestamp (milliseconds) (present only if type = `stop` or `stop_limit`)
trigger_price | string \| undefined | trigger price (present only if type = `stop` or `stop_limit`)
status | string | status enum: `INACTIVE`, `UNFILLED`, `PARTIALLY_FILLED`, `FULLY_FILLED`, `CANCELED_UNFILLED`, `CANCELED_PARTIALLY_FILLED`

**Caveat:**

* This API cannot fetch informations of an order that is executed or canceled more than 3 months ago. (it returns a 50009 error code.)
  Please use [the page of download order history](https://app.bitbank.cc/account/data/orders/download) instead for fetching those orders.

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
    "triggered_at": 0,
    "trigger_price": "string",
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
pair | string | YES | pair enum: [pair list](pairs.md)
amount | string | YES | amount
price | string | NO | price
side | string | YES | `buy` or `sell`
type | string | YES | `limit` or `market` or `stop` or `stop_limit`
post_only | boolean | NO | Post Only (`true` can be specified only if type = `limit`. default `false`)
trigger_price | string | NO | trigger price

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | order id
pair | string | pair enum: [pair list](pairs.md)
side | string | `buy` or `sell`
type | string | `limit` or `market` or `stop` or `stop_limit`
start_amount | string | order qty when placed
remaining_amount | string | qty not executed
executed_amount| string | qty executed
price | string \| undefined | order price (present only if type = `limit` or `stop_limit`)
post_only | boolean \| undefined | whether Post Only or not (present only if type = `limit`)
average_price | string | avg executed price
ordered_at | number | ordered at unix timestamp (milliseconds)
expire_at | number | expiration time in unix timestamp (milliseconds)
trigger_price | string \| undefined | trigger price (present only if type = `stop` or `stop_limit`)
status | string | status enum: `INACTIVE`, `UNFILLED`, `PARTIALLY_FILLED`, `FULLY_FILLED`, `CANCELED_UNFILLED`, `CANCELED_PARTIALLY_FILLED`

**Caveat:**
- Except for circuit_break_info.mode is `NONE`, market order are restricted. If restricted, it returns 70020 error code.
- `post_only` option is treated as `false` except, circuit_break_info.mode is `NONE`.

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
    "trigger_price": "string",
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
pair | string | YES | pair enum: [pair list](pairs.md)
order_id | number | YES | order id

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | order id
pair | string | pair enum: [pair list](pairs.md)
side | string | `buy` or `sell`
type | string | `limit` or `market` or `stop` or `stop_limit`
start_amount | string | order qty when placed
remaining_amount | string | qty not executed
executed_amount| string | qty executed
price | string \| undefined | order price (present only if type = `limit` or `stop_limit`)
post_only | boolean \| undefined | whether Post Only or not (present only if type = `limit`)
average_price | string | avg executed price
ordered_at | number | ordered at unix timestamp (milliseconds)
expire_at | number | expiration time in unix timestamp (milliseconds)
canceled_at | number | canceled at unix timestamp (milliseconds)
triggered_at | number \| undefined | triggered at unix timestamp (milliseconds) (present only if type = `stop` or `stop_limit`)
trigger_price | string \| undefined | trigger price (present only if type = `stop` or `stop_limit`)
status | string | status enum: `INACTIVE`, `UNFILLED`, `PARTIALLY_FILLED`, `FULLY_FILLED`, `CANCELED_UNFILLED`, `CANCELED_PARTIALLY_FILLED`

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
    "triggered_at": 0,
    "trigger_price": "string",
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
pair | string | YES | pair enum: [pair list](pairs.md)
order_ids | number[] | YES | order ids. Up to 30 ids can be specified

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
        "triggered_at": 0,
        "trigger_price": "string",
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
pair | string | YES | pair enum: [pair list](pairs.md)
order_ids | number[] | YES | order ids

**Caveat:**

* This API cannot fetch informations of orders that is executed or canceled more than 3 months ago. (it don't return any error nor those orders.)
  Please use [the page of download order history](https://app.bitbank.cc/account/data/orders/download) instead for fetching those orders.

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
        "triggered_at": 0,
        "trigger_price": "string",
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
pair | string | YES | pair enum: [pair list](pairs.md)
count | number | NO | take limit
from_id | number | NO | take from order id
end_id | number | NO | take until order id
since | number | NO | since unix timestamp
end | number | NO | end unix timestamp

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | order id
pair | string | pair enum: [pair list](pairs.md)
side | string | `buy` or `sell`
type | string | `limit` or `market` or `stop` or `stop_limit`
start_amount | string | order qty when placed
remaining_amount | string | qty not executed
executed_amount| string | qty executed
price | string \| undefined | order price (present only if type = `limit` or `stop_limit`)
post_only | boolean \| undefined | whether Post Only or not (present only if type = `limit`)
average_price | string | avg executed price
ordered_at | number | ordered at unix timestamp (milliseconds)
expire_at | number | expiration time in unix timestamp (milliseconds)
executed_at | number \| undefined | executed at unix timestamp (milliseconds)
canceled_at | number \| undefined | canceled at unix timestamp (milliseconds)
triggered_at | number \| undefined | triggered at unix timestamp (milliseconds) (present only if type = `stop` or `stop_limit`)
trigger_price | string \| undefined | trigger price (present only if type = `stop` or `stop_limit`)
status | string | status enum: `INACTIVE`, `UNFILLED`, `PARTIALLY_FILLED`, `FULLY_FILLED`, `CANCELED_UNFILLED`, `CANCELED_PARTIALLY_FILLED`

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
        "triggered_at": 0,
        "trigger_price": "string",
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
pair | string | YES | pair enum: [pair list](pairs.md)
count | number | NO | take limit (up to 1000)
order_id | number | NO | order id
since | number | NO | since unix timestamp
end | number | NO | end unix timestamp
order | string | NO | histories in order(order enum: `asc` or `desc`, default to `desc`)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
trade_id | number | trade id
pair | string | pair enum: [pair list](pairs.md)
order_id | number | order id
side | string | `buy` or `sell`
type | string | `limit` or `market` or `stop` or `stop_limit`
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

### Deposit

#### Fetch deposit history

```txt
GET /user/deposit_history
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
asset | string | YES | enum: [asset list](assets.md)
count | number | NO | take limit (up to 100)
since | number | NO | since unix timestamp
end | number | NO | end unix timestamp

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | uuid for each deposit
address | string | deposit address
asset | string | enum: [asset list](assets.md)
network | string | enum: [network list](networks.md)
amount | number | deposit amount
txid | string \| null | deposit transaction id (only for crypto assets)
status | string | deposit status enum: `FOUND`, `CONFIRMED`, `DONE`
found_at | number| found at unix timestamp (milliseconds)
confirmed_at | number | confirmed (about to be added to your balance) at unix timestamp (milliseconds, exists only for confirmed one)


**Caveat:**

* The deposit history response currently does not contain destination tag, memo and bank account. Use txid for matching asset flows with other systems.

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/deposit_history?asset=btc" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/deposit_history?asset=btc
```

</p>
</details>


**Response format:**

```json
{
  "success": 1,
  "data": {
    "deposits": [
      {
        "uuid": "string",
        "asset": "string",
        "network": "string",
        "amount": "string",
        "txid": "string",
        "status": "string",
        "found_at": 0,
        "confirmed_at": 0
      }
    ]
  }
}
```

#### Fetch unconfirmed deposits

```txt
GET /user/unconfirmed_deposits
```

**Parameters:**
None

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | deposit uuid
asset | string | enum: [asset list](assets.md)
amount | string | deposit amount
network | string | deposit network (only for crypto assets)
txid | string | null | deposit transaction id (only for crypto assets)
created_at | number| created at unix timestamp (milliseconds)

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/unconfirmed_deposits" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/unconfirmed_deposits
```

</p>
</details>


**Response format:**

```json
{
  "success": 1,
  "data": {
    "deposits": [
      {
        "uuid": "string",
        "asset": "string",
        "amount": "string",
        "network": "string",
        "txid": "string",
        "created_at": 0
      }
    ]
  }
}
```

#### Fetch deposit originators

```txt
GET /user/deposit_originators
```

**Parameters:**
None

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | originator uuid
label | string | originator label
deposit_type | string | deposit type
deposit_purpose | string | null | deposit purpose
originator_status | string | originator status
originator_type | string | originator type
originator_last_name | string | null | originator last name
originator_first_name | string | null | originator first name
originator_country | string | null | originator country
originator_prefecture | string | null | originator prefecture/state/province/region
originator_city | string | null | originator city
originator_address | string | null | originator address
originator_building | string | null | originator building
originator_company_name | string | null | originator company name
originator_company_type | string | null | originator company type
originator_company_type_position | null | string | originator company type position
uuid | string | originator substantial controller uuid
name | string | originator substantial controller name
country | string | originator substantial controller country
prefecture | string | null | originator substantial controller prefecture

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/deposit_originators" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/deposit_originators
```

</p>
</details>


**Response format:**

```json
{
  "success": 1,
  "data": {
    "originators": [
      {
        "uuid": "string",
        "label": "string",
        "deposit_type": "string",
        "deposit_purpose": "string",
        "originator_status": "string",
        "originator_type": "string",
        "originator_last_name": null,
        "originator_first_name": null,
        "originator_country": "string",
        "originator_prefecture": "string",
        "originator_city": "string",
        "originator_address": "string",
        "originator_building": null,
        "originator_company_name": "string",
        "originator_company_type": "string",
        "originator_company_type_position": "string",
        "originator_substantial_controllers": [
            {
                "uuid": "string",
                "name": "string",
                "country": "string",
                "prefecture": null
            }
        ]
      }
    ]
  }
}
```

#### Confirm deposits

```txt
POST /user/confirm_deposits
```

**Parameters (requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
uuid | string | YES | unconfirmed deposit uuid
originator_uuid | string | YES | originator uuid

**Response:**
None

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export REQUEST_BODY='{"deposits": [{ "uuid": "___deposit uuid___", "originator_uuid": "___originator uuid___" }]}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' -H "Content-Type: application/json" -d ''"$REQUEST_BODY"'' https://api.bitbank.cc/v1/user/confirm_deposits
```

</p>
</details>


**Response format:**

```json
{
  "success": 1,
  "data": {}
}
```

#### Confirm all deposits

```txt
POST /user/confirm_deposits_all
```

**Parameters (requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
originator_uuid | string | YES | originator uuid

**Response:**
None

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export REQUEST_BODY='{"originator_uuid": "___originator uuid___"}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' -H "Content-Type: application/json" -d ''"$REQUEST_BODY"'' https://api.bitbank.cc/v1/user/confirm_deposits_all
```

</p>
</details>


**Response format:**

```json
{
  "success": 1,
  "data": {}
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
asset | string | YES | asset enum: [asset list](assets.md)

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

#### New withdrawal request

```txt
POST /user/request_withdrawal
```

**Parameters (requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
asset | string | YES | enum: [asset list](assets.md)
uuid | string | YES | withdrawal account's uuid
amount | string | YES | withdrawal amount
otp_token | string | NO | provide if MFA is set up
sms_token | string | NO | provide if MFA is set up

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | uuid for each withdrawal
asset | string | enum: [asset list](assets.md)
account_uuid | string | account uuid
amount | string | withdrawal amount
fee | string | withdrawal fee
label | string | withdrawal account label (only for crypto assets)
address | string | withdrawal destination address (only for crypto assets)
network | string | enum (only for crypto assets): [network list](networks.md)
destination_tag | number or string | withdrawal destination tag or memo (only for crypto that have it)
txid | string \| null | withdrawal transaction id (only for crypto assets)
bank_name | string | bank of withdrawal account (only for fiat assets)
branch_name | string | bank branch of withdrawal account (only for fiat assets)
account_type | string | type of withdrawal account (only for fiat assets)
account_number | string | withdrawal account number (only for fiat assets)
account_owner | string | owner of withdrawal account (only for fiat assets)
status | string | withdrawal status enum: `CONFIRMING`, `EXAMINING`, `SENDING`,  `DONE`, `REJECTED`, `CANCELED`, `CONFIRM_TIMEOUT`
requested_at | number | requested at unix timestamp (milliseconds)


**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export REQUEST_BODY='{"asset": "xrp", "uuid": "___your uuid___", "amount": "1"}'
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
    "account_uuid": "string",
    "amount": "string",
    "fee": "string",

    "label": "string",
    "address": "string",
    "network": "string",
    "txid": "string",
    "destination_tag": 0,

    "bank_name": "string",
    "branch_name": "string",
    "account_type": "string",
    "account_number": "string",
    "account_owner": "string",

    "status": "string",
    "requested_at": 0
  }
}
```

#### Fetch withdrawal history

```txt
GET /user/withdrawal_history
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
asset | string | YES | enum: [asset list](assets.md)
count | number | NO | take limit (up to 100)
since | number | NO | since unix timestamp
end | number | NO | end unix timestamp

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | uuid for each withdrawal
asset | string | enum: [asset list](assets.md)
account_uuid | string | account uuid
amount | number | withdrawal amount
fee | number | withdrawal fee
label | string | withdrawal account label (only for crypto assets)
address | string | withdrawal destination address (only for crypto assets)
network | string | enum (only for crypto assets): [network list](networks.md)
destination_tag | number or string | withdrawal destination tag or memo (only for crypto that have it)
txid | string \| null | withdrawal transaction id (only for crypto assets)
bank_name | string | bank of withdrawal account (only for fiat assets)
branch_name | string | bank branch of withdrawal account (only for fiat assets)
account_type | string | type of withdrawal account (only for fiat assets)
account_number | string | withdrawal account number (only for fiat assets)
account_owner | string | owner of withdrawal account (only for fiat assets)
status | string | withdrawal status enum: `CONFIRMING`, `EXAMINING`, `SENDING`,  `DONE`, `REJECTED`, `CANCELED`, `CONFIRM_TIMEOUT`
requested_at | number | requested at unix timestamp (milliseconds)


**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/withdrawal_history?asset=btc" | openssl dgst -sha256 -hmac "$API_SECRET")"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/withdrawal_history?asset=btc
```

</p>
</details>


**Response format:**

```json
{
  "success": 1,
  "data": {
    "withdrawals": [
      {
        "uuid": "string",
        "asset": "string",
        "account_uuid": "string",
        "amount": "string",
        "fee": "string",

        "label": "string",
        "address": "string",
        "network": "string",
        "txid": "string",
        "destination_tag": 0,

        "bank_name": "string",
        "branch_name": "string",
        "account_type": "string",
        "account_number": "string",
        "account_owner": "string",

        "status": "string",
        "requested_at": 0
      }
    ]
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
pair | string | pair enum: [pair list](pairs.md)
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
name | string | pair enum: [pair list](pairs.md)
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
stop_market_order | boolean | "market order" suspended flag
stop_stop_order | boolean | "stop (market) order" suspended flag
stop_stop_limit_order | boolean | "stop limit order" suspended flag
stop_buy_order | boolean | "buy order" suspended flag
stop_sell_order | boolean | "sell order" suspended flag

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
        "stop_order_and_cancel": false,
        "stop_market_order": false,
        "stop_stop_order": false,
        "stop_stop_limit_order": false,
        "stop_buy_order": false,
        "stop_sell_order": false
      }
    ]
  }
}
```
