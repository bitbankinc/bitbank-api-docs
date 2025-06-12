[日本語](private-stream_JP.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Private stream for bitbank](#private-stream-for-bitbank)
  - [Introduction](#introduction)
    - [What is a private stream?](#what-is-a-private-stream)
    - [What is PubNub?](#what-is-pubnub)
  - [How to use](#how-to-use)
    - [Step 1: Get your API key](#step-1-get-your-api-key)
    - [Step 2: Get channel name and token](#step-2-get-channel-name-and-token)
    - [Step 3: Connect to PubNub](#step-3-connect-to-pubnub)
    - [Step 4: Receive order user data](#step-4-receive-order-user-data)
      - [method: asset_update](#method-asset_update)
      - [method: spot_order_new](#method-spot_order_new)
      - [method: spot_order](#method-spot_order)
      - [method: spot_order_invalidation](#method-spot_order_invalidation)
      - [method: spot_trade](#method-spot_trade)
      - [method: dealer_order_new](#method-dealer_order_new)
      - [method: withdrawal](#method-withdrawal)
      - [method: deposit](#method-deposit)
      - [method: margin_position_update](#method-margin_position_update)
      - [method: margin_payable_update](#method-margin_payable_update)
      - [method: margin_notice_update](#method-margin_notice_update)
    - [Step 5: Reconnect to PubNub](#step-5-reconnect-to-pubnub)
  - [Example code](#example-code)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Private stream for bitbank

## Introduction

### What is a private stream?

A private stream is a feature that allows you to receive real-time user data (such as order and asset change information) from the bitbank server.
This feature is useful when you want to trade using real-time user data.
User data is sent using PubNub and can be received in real-time.

### What is PubNub?

PubNub is a service that provides real-time data streaming. bitbank uses PubNub to provide users with real-time user data.

Using the PubNub SDK, you can connect to the PubNub server based on the information obtained from the API and receive user data.

## How to use

### Step 1: Get your API key

To use the private stream, please get your API key and secret from the bitbank website.

### Step 2: Get channel name and token

To connect to the private stream, you need to get the channel name and token from REST API GET /v1/user/subscribe endpoint.

Refer to ["Get channel and token for private stream"](rest-api.md#get-channel-and-token-for-private-stream) in [rest-api](rest-api.md).

### Step 3: Connect to PubNub

To connect to the private stream, you need to use the PubNub SDK. You can connect to the private stream by following the steps below:

1. Install the [PubNub SDK](https://www.pubnub.com/docs/sdks) for your language. ex. JavaScript: `npm install pubnub`
2. Create a PubNub configuration. ex. [JavaScript](https://www.pubnub.com/docs/sdks/javascript/api-reference/configuration)
3. Receive user data from the private stream. ex. [JavaScript](https://www.pubnub.com/docs/sdks/javascript/api-reference/publish-and-subscribe#subscribe)

Use `sub-c-ecebae8e-dd60-11e6-b6b1-02ee2ddab7fe` as the subscribeKey.

Refer to the [Example code](#example-code) for detailed implementation.

### Step 4: Receive order user data

Once you have connected to the private stream, you can start receiving user data. The user data is sent in JSON format and contains the following information:

#### method: asset_update

Receive information about changes in owned assets.

Name | Type | Description
------------ | ------------ | ------------
asset | string  | Asset name: [Asset list](assets.md)
amount_precision | number | Precision
free_amount | string | Available amount
locked_amount | string | Locked amount
onhand_amount | string | On-hand amount
withdrawing_amount | string | Withdrawing amount

**Response format:**

```json
{
  "message": {
    "method": "asset_update",
    "params": [
      {
        "asset": "string",
        "amountPrecision": 0,
        "freeAmount": "string",
        "lockedAmount": "string",
        "onhandAmount": "string",
        "withdrawingAmount": "string"
      }
    ]
  }
}
```

#### method: spot_order_new

Receive information about new orders.

If you are managing active orders locally, when you receive a notification with the status `FULLY_FILLED`, `CANCELED_UNFILLED`, or `CANCELED_PARTIALLY_FILLED`, it indicates that the order has been executed or canceled, so please remove it from your order information.

Name | Type | Description
------------ | ------------ | ------------
average_price | string | avg executed price
canceled_at | number | canceled at unix timestamp (milliseconds)
executed_amount | string | qty executed
executed_at | number | order executed at unix timestamp (milliseconds)
order_id | number | order ID
ordered_at | number | ordered at unix timestamp (milliseconds)
pair | string | pair enum: [pair list](pairs.md)
price | string | order price
trigger_price | string \| undefined | trigger price(present only if type = `stop`, `stop_limit`, `take_profit`, `stop_loss`)
remaining_amount | string \| null | qty not executed
position_side | string \| undefined | `long` or `short`(only for margin trading)
side | string | `buy` or `sell`
start_amount | string \| null | order qty when placed
status | string | status enum: `INACTIVE`, `UNFILLED`, `PARTIALLY_FILLED`, `FULLY_FILLED`, `CANCELED_UNFILLED`, `CANCELED_PARTIALLY_FILLED`
type | string | one of `limit`, `market`, `stop`, `stop_limit`, `take_profit`, `stop_loss`, `losscut`
expire_at | number \| null | expiration time in unix timestamp (milliseconds)
triggered_at | number \| undefined | triggered at unix timestamp (milliseconds) (present only if type = `stop`, `stop_limit`, `take_profit`, `stop_loss`)
post_only | boolean \| undefined | whether Post Only or not (present only if type = `limit`)
user_cancelable | boolean | User cancelable
is_just_triggered | boolean | Just triggered

**Response format:**

```json
{
  "message": {
    "method": "spot_order_new",
    "params": [
      {
        "average_price": "string",
        "canceled_at": 0,
        "executed_amount": "string",
        "executed_at": 0,
        "order_id": 0,
        "ordered_at": 0,
        "pair": "string",
        "price": "string",
        "trigger_price": "string",
        "remaining_amount": "string",
        "position_side": "string",
        "side": "string",
        "start_amount": "string",
        "status": "string",
        "type": "string",
        "expire_at": 0,
        "triggered_at": 0,
        "post_only": false,
        "user_cancelable": true,
        "is_just_triggered": false
      }
    ]
  }
}
```

#### method: spot_order

Receive information about order updates.

The content is the same as `spot_order_new`.

If you are managing active orders locally, when you receive a notification with the status `FULLY_FILLED`, `CANCELED_UNFILLED`, or `CANCELED_PARTIALLY_FILLED`, it indicates that the order has been executed or canceled, so please remove it from your order information.

**Response format:**

```json
{
  "message": {
    "method": "spot_order",
    "params": [
      {
        "average_price": "string",
        "canceled_at": 0,
        "executed_amount": "string",
        "executed_at": 0,
        "order_id": 0,
        "ordered_at": 0,
        "pair": "string",
        "price": "string",
        "trigger_price": "string",
        "remaining_amount": "string",
        "position_side": "string",
        "side": "string",
        "start_amount": "string",
        "status": "string",
        "type": "string",
        "expire_at": 0,
        "triggered_at": 0,
        "post_only": false,
        "user_cancelable": true,
        "is_just_triggered": false
      }
    ]
  }
}
```

#### method: spot_order_invalidation

Receive information about order invalidation.

This notification is sent when an order is invalidated due to asset shortages within our matching engine.
Therefore, this notification does not occur frequently.
Immediate cancellations of orders that typically occur are notified via `spot_order_new`, `spot_order`, or error responses from the REST API.

If you are managing active orders locally, please remove the notified order from your order information.

Name | Type | Description
------------ | ------------ | ------------
order_id | number | order ID

**Response format:**

```json
{
  "message": {
    "method": "spot_order_invalidation",
    "params": {
      "order_id": [1, 2, 3]
    }
  }
}
```

#### method: spot_trade

Receive information about trades.

Name | Type | Description
------------ | ------------ | ------------
amount | string | executed amount
executed_at | number | order executed at unix timestamp (milliseconds)
fee_amount_base | string | base asset fee amount
fee_amount_quote | string | quote asset fee amount
fee_occurred_amount_quote | string | Quote fee occurred. Collected on close orders for margin trading. For spot trading, it is collected immediately and is the same as `fee_amount_quote`.
maker_taker | string | maker or taker
order_id | number | order ID
pair | string | pair enum: [pair list](pairs.md)
price | string | order price
position_side | string \| undefined | `long` or `short`(only for margin trading)
side | string | `buy` or `sell`
trade_id | number | trade ID
type | string | one of `limit`, `market`, `stop`, `stop_limit`, `take_profit`, `stop_loss`, `losscut`
profit_loss | string \| undefined | realized profit and loss
interest | string \| undefined | interest

**Response format:**

```json
{
  "message": {
    "method": "spot_trade",
    "params": [
      {
        "amount": "string",
        "executed_at": 0,
        "fee_amount_base": "string",
        "fee_amount_quote": "string",
        "fee_occurred_amount_quote": "string",
        "maker_taker": "string",
        "order_id": 0,
        "pair": "string",
        "price": "string",
        "position_side": "string",
        "side": "string",
        "trade_id": 0,
        "type": "string",
        "profit_loss": "string",
        "interest": "string"
      }
    ]
  }
}
```

#### method: dealer_order_new

Receive information about new dealer orders.

Name | Type | Description
------------ | ------------ | ------------
order_id | string | order ID
asset | string | enum: [asset list](assets.md)
side | string | `buy` or `sell`
price | string | price
amount | string | amount
ordered_at | number | ordered at unix timestamp (milliseconds)

**Response format:**

```json
{
  "message": {
    "method": "dealer_order_new",
    "params": [
      {
        "order_id": 0,
        "asset": "string",
        "side": "string",
        "price": "string",
        "amount": "string",
        "ordered_at": 0
      }
    ]
  }
}
```

#### method: withdrawal

Receive information about withdrawals.

Name | Type | Description
------------ | ------------ | ------------
uuid | string | withdrawal ID
network | string | enum: [network list](networks.md)(only for crypto assets)
asset | string | enum: [asset list](assets.md)
account_uuid | string | account UUID
asset | string | enum: [asset list](assets.md)
fee | string | withdrawal fee
label | string | withdrawal account label (only for crypto assets)
address | string | withdrawal destination address (only for crypto assets)
txid | string \| null | withdrawal transaction id (only for crypto assets)
bank_name | string | bank of withdrawal account (only for fiat assets)
branch_name | string | bank branch of withdrawal account (only for fiat assets)
account_type | string | type of withdrawal account (only for fiat assets)
account_number | string | withdrawal account number (only for fiat assets)
account_owner | string | owner of withdrawal account (only for fiat assets)
status | string | withdrawal status enum: `CONFIRMING`, `EXAMINING`, `SENDING`,  `DONE`, `REJECTED`, `CANCELED`, `CONFIRM_TIMEOUT`
requested_at | number | requested at unix timestamp (milliseconds)

**Response format:**

```json
{
  "message": {
    "method": "withdrawal",
    "params": [
      {
        "uuid": "string",
        "asset": "string",
        "account_uuid": "string",
        "amount": "string",
        "fee": "string",
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

#### method: deposit

Receive information about deposits.

Name | Type | Description
------------ | ------------ | ------------
uuid | string | deposit ID
asset | string | enum: [asset list](assets.md)
amount | number | deposit amount
network | string | enum: [network list](networks.md)(only for crypto assets)
address | string | deposit address(only for crypto assets)
txid | string \| null | deposit transaction id (only for crypto assets)
found_at | number | found at unix timestamp (milliseconds)
confirmed_at | number | confirmed (about to be added to your balance) at unix timestamp (milliseconds, exists only for confirmed one)
status | string | deposit status enum: `FOUND`, `CONFIRMED`, `DONE`

**Response format:**

```json
{
  "message": {
    "method": "deposit",
    "params": [
      {
        "uuid": "string",
        "asset": "string",
        "amount": "string",
        "network": "string",
        "address": "string",
        "txid": "string",
        "found_at": 0,
        "confirmed_at": 0,
        "status": "string"
      }
    ]
  }
}
```

#### method: margin_position_update

Receive information about margin positions.

Name | Type | Description
------------ | ------------ | ------------
pair | string | pair enum: [pair list](pairs.md)
position_side | string | `long` or `short`
average_price | string | avg executed price
open_amount | string | open amount
locked_amount | string | locked amount
product | string | average executed price x open amount
unrealized_fee_amount | string | unrealized fee amount
unrealized_interest_amount | string | unrealized interest amount

**Response format:**

```json
{
  "message": {
    "method": "margin_position_update",
    "params": [
      {
        "pair": "string",
        "position_side": "string",
        "average_price": "string",
        "open_amount": "string",
        "locked_amount": "string",
        "product": "string",
        "unrealized_fee_amount": "string",
        "unrealized_interest_amount": "string"
      }
    ]
  }
}
```

#### method: margin_payable_update

Receive information about margin payables.

Name | Type | Description
------------ | ------------ | ------------
amount | string | payable amount

**Response format:**

```json
{
  "message": {
    "method": "margin_payable_update",
    "params": [
      {
        "amount": "string"
      }
    ]
  }
}
```

#### method: margin_notice_update

Receive information about margin notices.

This is published when a margin call or insufficient funds occur, or when they are partially deposited or partially repaid.
Additionally, if these are resolved or fully settled, a message with all properties set to `null` will be published.

Name | Type | Description
------------ | ------------ | ------------
what | string \| null | what the notice is for (marginCall, debt, or settled)
occurred_at | number \| null | occurrence time
amount | string \| null | amount
due_date_at | number \| null | due date

**Response format:**

```json
{
  "message": {
    "method": "margin_notice_update",
    "params": [
      {
        "what": "string",
        "occurred_at": 0,
        "amount": "string",
        "due_date_at": 0
      }
    ]
  }
}
```

### Step 5: Reconnect to PubNub

If the connection to the private stream is lost, you can reconnect to PubNub by following the steps below:

1. Detect that the connection to PubNub has been lost from the status event.
2. Reconnect to PubNub.

Refer to the [Example code](#example-code) for detailed implementation.

## Example code

Here is an example code that shows how to connect to the private stream and receive user data:

```javascript
// npm install pubnub
const PubNub = require('pubnub');
const crypto = require('crypto');

const API_KEY = '<your_api_key>';
const API_SECRET = '<your_api_secret>';

const PUBNUB_SUB_KEY = 'sub-c-ecebae8e-dd60-11e6-b6b1-02ee2ddab7fe';

const store = new Map();
// status only changes in the direction from 0 to 3
const STATUS_STAGE = {
    INACTIVE: 0,
    UNFILLED: 1,
    PARTIALLY_FILLED: 2,
    FULLY_FILLED: 3,
    CANCELED_PARTIALLY_FILLED: 3,
    CANCELED_UNFILLED: 3,
}

const main = async () => {

    const { channel, token } = await getChannelAndToken();

    const pubnub = getPubNubAndAddListener(channel, token);

    subscribePrivateChannel(pubnub, channel, token);

    // If the status becomes 3 (inactive), remove it from the store after 1 minute
    setInterval(() => {
      const now = new Date().getTime();
      store.forEach((value, key) => {
        if (now - value.last_update_at > 60 * 1000 && STATUS_STAGE[value.status] === 3) {
          store.delete(key);
        }
      });

      console.log('store:', store);
    }, 60 * 1000);
}

const toSha256 = (key, value) =>{
  return crypto
    .createHmac('sha256', key)
    .update(Buffer.from(value))
    .digest('hex')
    .toString();
}

const getPubNubAndAddListener = (channel, token) => {
  const pubnub = new PubNub({
    subscribeKey: PUBNUB_SUB_KEY,
    userId: channel,
    ssl: true,
  });
  /**
   * Note: In some SDKs, there are separate message receiving methods on the subscription instance in addition to addListener.
   *       Using both may result in messages being delivered to only one handler.
   *       It is recommended to handle both status and message events within addListener.
   *
   * The following are not recommended:
   * JavaScript: subscription.onMessage
   * Java: subscription.setOnMessage
   * C#: subscription.OnMessage
   * Ruby: subscription.on_message
   * etc.
   */
  pubnub.addListener({
    status: async (status) => {
      switch (status.category) {
        /**
         * When pubnub channel connection established successfully.
         * This is called once after trying to start to connect.
         */
        case 'PNConnectedCategory': {
          console.info('pubnub connection established');
          break;
        }

        /**
         * When connection restored.
         */
        case 'PNNetworkUpCategory':
        case 'PNReconnectedCategory': {
          console.info('pubnub connection restored');
          // When network down, pubnub disposes subscribers so we need to re-subscribe manually.
          subscribePrivateChannel(pubnub_channel, token);
          break;
        }

        /**
         * When connection failed by timeout.
         * In this case, we need to reconnect manually.
         */
        case 'PNTimeoutCategory': {
          console.warn('pubnub connection Failed by timeout.');
          await reconnect();
          break;
        }

        /**
         * When connection failed due to network error.
         */
        case 'PNNetworkDownCategory':
        case 'PNNetworkIssuesCategory': {
          console.error('pubnub connection network error', status);
          await reconnect();
          break;
        }

        /**
         * This will be called when token is expired.
         */
        case 'PNAccessDeniedCategory': {
          console.error('pubnub access denied', status);
          await reconnect();
          break;
        }

        /**
         * This will not be called.
         * Maybe pubnub-library is not implement these events.
         */
        case 'PNBadRequestCategory':
        case 'PNMalformedResponseCategory':
        case 'PNDecryptionErrorCategory': {
          console.error('pubnub connection failed', status);
          break;
        }

        /**
         * This will not be called.
         * If this called, implement new handling for the event.
         */
        default: {
          console.warn('default');
        }
      }
    },

    // Since message order is not guaranteed, only the most up-to-date order status should remain in the store, even if order messages arrive out of sequence.
    // When an order message arrives, check the store: if it doesn't exist, add it; if it does, update only if the order status has progressed.
    message: (data) => {
      if (data && data.message) {
        console.info('message received', JSON.stringify(data.message));
      }
      if (data.message.method === 'spot_order_new' || data.message.method === 'spot_order') {
        const order = data.message.params[0];
        if (!store.has(order.order_id)) {
          store.set(order.order_id, {
            status: order.status,
            remaining_amount: order.remaining_amount,
            last_update_at: new Date().getTime(),
          });
        } else {
          const last = store.get(order.order_id);
          if (STATUS_STAGE[last.status] < STATUS_STAGE[order.status] || (STATUS_STAGE[last.status] === STATUS_STAGE[order.status] && last.remaining_amount > order.remaining_amount)) {
            store.set(order.order_id, {
              status: order.status,
              remaining_amount: order.remaining_amount,
              last_update_at: new Date().getTime(),
            });
          }
        }
      }
    },
  });

  return pubnub;
}

const getChannelAndToken = async () => {
  const nonce = new Date().getTime();
  const timeWindow = '5000';
  const message = `${nonce}${timeWindow}/v1/user/subscribe`;

  // Get channel and token from API
  const res = await fetch('https://api.bitbank.cc/v1/user/subscribe', {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
      'ACCESS-KEY': API_KEY,
      'ACCESS-REQUEST-TIME': nonce.toString(),
      'ACCESS-TIME-WINDOW': timeWindow,
      'ACCESS-SIGNATURE': toSha256(API_SECRET, message),
    },
  }).then((res) => res.json());

  const channel = res.data.pubnub_channel;
  const token = res.data.pubnub_token;
  return { channel, token };
}

const subscribePrivateChannel = (pubnub, channel, token) => {
  pubnub.setToken(token);
  pubnub.subscribe({
    channels: [channel],
  });
}

const reconnect = async () => {
  // Reconnect to pubnub
  const { channel, token } = await getChannelAndToken();
  const pubnub = getPubNubAndAddListener(channel, token);

  subscribePrivateChannel(pubnub, channel, token);
}

main().catch(console.error);
```