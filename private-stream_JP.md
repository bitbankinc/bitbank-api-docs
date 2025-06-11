[English](private-stream.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [プライベートストリーム](#%E3%83%97%E3%83%A9%E3%82%A4%E3%83%99%E3%83%BC%E3%83%88%E3%82%B9%E3%83%88%E3%83%AA%E3%83%BC%E3%83%A0)
  - [はじめに](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)
    - [プライベートストリームとは？](#%E3%83%97%E3%83%A9%E3%82%A4%E3%83%99%E3%83%BC%E3%83%88%E3%82%B9%E3%83%88%E3%83%AA%E3%83%BC%E3%83%A0%E3%81%A8%E3%81%AF)
    - [PubNubとは？](#pubnub%E3%81%A8%E3%81%AF)
  - [使い方](#%E4%BD%BF%E3%81%84%E6%96%B9)
    - [ステップ1: APIキーを取得する](#%E3%82%B9%E3%83%86%E3%83%83%E3%83%971-api%E3%82%AD%E3%83%BC%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
    - [ステップ2: チャンネル名とトークンを取得する](#%E3%82%B9%E3%83%86%E3%83%83%E3%83%972-%E3%83%81%E3%83%A3%E3%83%B3%E3%83%8D%E3%83%AB%E5%90%8D%E3%81%A8%E3%83%88%E3%83%BC%E3%82%AF%E3%83%B3%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
    - [ステップ3: PubNubに接続する](#%E3%82%B9%E3%83%86%E3%83%83%E3%83%973-pubnub%E3%81%AB%E6%8E%A5%E7%B6%9A%E3%81%99%E3%82%8B)
    - [ステップ4: ユーザーデータを受信する](#%E3%82%B9%E3%83%86%E3%83%83%E3%83%974-%E3%83%A6%E3%83%BC%E3%82%B6%E3%83%BC%E3%83%87%E3%83%BC%E3%82%BF%E3%82%92%E5%8F%97%E4%BF%A1%E3%81%99%E3%82%8B)
      - [資産変更通知 asset_update](#%E8%B3%87%E7%94%A3%E5%A4%89%E6%9B%B4%E9%80%9A%E7%9F%A5-asset_update)
      - [新規注文通知 spot_order_new](#%E6%96%B0%E8%A6%8F%E6%B3%A8%E6%96%87%E9%80%9A%E7%9F%A5-spot_order_new)
      - [注文変更通知 spot_order](#%E6%B3%A8%E6%96%87%E5%A4%89%E6%9B%B4%E9%80%9A%E7%9F%A5-spot_order)
      - [注文無効通知 spot_order_invalidation](#%E6%B3%A8%E6%96%87%E7%84%A1%E5%8A%B9%E9%80%9A%E7%9F%A5-spot_order_invalidation)
      - [約定情報通知 spot_trade](#%E7%B4%84%E5%AE%9A%E6%83%85%E5%A0%B1%E9%80%9A%E7%9F%A5-spot_trade)
      - [新規販売所注文情報通知 dealer_order_new](#%E6%96%B0%E8%A6%8F%E8%B2%A9%E5%A3%B2%E6%89%80%E6%B3%A8%E6%96%87%E6%83%85%E5%A0%B1%E9%80%9A%E7%9F%A5-dealer_order_new)
      - [出金情報通知 withdrawal](#%E5%87%BA%E9%87%91%E6%83%85%E5%A0%B1%E9%80%9A%E7%9F%A5-withdrawal)
      - [入金情報通知 deposit](#%E5%85%A5%E9%87%91%E6%83%85%E5%A0%B1%E9%80%9A%E7%9F%A5-deposit)
      - [建玉ポジション情報通知 margin_position_update](#%E5%BB%BA%E7%8E%89%E3%83%9D%E3%82%B8%E3%82%B7%E3%83%A7%E3%83%B3%E6%83%85%E5%A0%B1%E9%80%9A%E7%9F%A5-margin_position_update)
      - [決済損金額・不足金額情報通知 margin_payable_update](#%E6%B1%BA%E6%B8%88%E6%90%8D%E9%87%91%E9%A1%8D%E3%83%BB%E4%B8%8D%E8%B6%B3%E9%87%91%E9%A1%8D%E6%83%85%E5%A0%B1%E9%80%9A%E7%9F%A5-margin_payable_update)
      - [督促情報通知 margin_notice_update](#%E7%9D%A3%E4%BF%83%E6%83%85%E5%A0%B1%E9%80%9A%E7%9F%A5-margin_notice_update)
    - [ステップ5: PubNubに再接続する](#%E3%82%B9%E3%83%86%E3%83%83%E3%83%975-pubnub%E3%81%AB%E5%86%8D%E6%8E%A5%E7%B6%9A%E3%81%99%E3%82%8B)
  - [サンプルコード](#%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB%E3%82%B3%E3%83%BC%E3%83%89)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# プライベートストリーム

## はじめに

### プライベートストリームとは？

プライベートストリームは、bitbankのサーバーからリアルタイムのユーザーデータ（注文、資産などの変更情報）を受信する機能です。
この機能は、リアルタイムにユーザーデータを用いた取引を行いたい場合に便利です。
ユーザーデータはPubNubを使用して送信され、リアルタイムで受信できます。

### PubNubとは？

PubNubは、リアルタイムデータストリーミングを提供するサービスです。bitbankは、ユーザーにリアルタイムのユーザーデータを提供するためにPubNubを使用しています。

PubNubのSDKを使用して、APIから取得した情報をもとに、PubNubサーバーに接続してユーザーデータを受信します。

## 使い方

### ステップ1: APIキーを取得する

プライベートストリームを使用するには、bitbankのウェブサイトからAPIキーとシークレットを取得してください。

### ステップ2: チャンネル名とトークンを取得する

プライベートストリームに接続するには、REST APIのGET /v1/user/subscribeエンドポイントよりチャンネル名とトークンを取得する必要があります。

[rest-api_JP](rest-api_JP.md)の [「プライベートストリームのチャンネルとトークンを取得する」](rest-api_JP.md#プライベートストリームのチャンネルとトークンを取得する)を参照してください。

### ステップ3: PubNubに接続する

プライベートストリームに接続するには、PubNub SDKを使用する必要があります。以下の手順に従ってプライベートストリームに接続できます。

1. 使用言語の[PubNub SDK](https://www.pubnub.com/docs/sdks)をインストールします。 ex. JavaScript: `npm install pubnub`
2. それぞれの言語のPubNub Configurationを行います。 ex. [JavaScript](https://www.pubnub.com/docs/sdks/javascript/api-reference/configuration)
3. subscribeを行います。 ex. [JavaScript](https://www.pubnub.com/docs/sdks/javascript/api-reference/publish-and-subscribe#subscribe)

subscribeKeyは、 `sub-c-ecebae8e-dd60-11e6-b6b1-02ee2ddab7fe` を使用してください。

詳細な実装は[サンプルコード](#サンプルコード)を参照してください。

### ステップ4: ユーザーデータを受信する

プライベートストリームに接続したら、ユーザーデータを受信できます。ユーザーデータはJSON形式で送信され、以下の情報を含んでいます。

#### 資産変更通知 asset_update

所有資産の変更情報を受信します。

Name | Type | Description
------------ | ------------ | ------------
asset | string  | アセット名: [アセット一覧](assets.md)
amount_precision | number | 精度
free_amount | string | 利用可能な量
locked_amount | string | ロックされている量
onhand_amount | string | 保有量
withdrawing_amount | string | ロックされている量のうち出金中数量

**レスポンスのフォーマット:**

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

#### 新規注文通知 spot_order_new

新規注文情報を受信します。

手元でアクティブ注文を管理されている場合は、statusが `FULLY_FILLED`、 `CANCELED_UNFILLED`、 `CANCELED_PARTIALLY_FILLED` の通知を受け取った時、注文が約定、あるいはキャンセルされることを示しているため、手元の注文情報から削除してください。


Name | Type | Description
------------ | ------------ | ------------
average_price | string | 平均約定価格
canceled_at | number \| undefined | キャンセル日時(UnixTimeのミリ秒)
executed_amount | string | 約定済み数量
executed_at | number | 約定日時（UnixTimeのミリ秒）
order_id | number | 注文ID
ordered_at | number | 注文日時(UnixTimeのミリ秒)
pair | string | 通貨ペア: [ペア一覧](pairs.md)
price | string \| undefined | 注文価格（type = `limit` または `stop_limit` 時のみ）
trigger_price | string \| undefined | トリガー価格（type = `stop`, `stop_limit`, `take_profit`, `stop_loss` 時のみ）
remaining_amount | string | 未約定の数量
position_side | string \| undefined | `long` または `short`（信用取引の時のみ）
side | string | `buy` または `sell`
start_amount | string | 注文時の数量
status | string | 注文ステータス: `INACTIVE` 非アクティブ, `UNFILLED` 注文中, `PARTIALLY_FILLED` 注文中(一部約定), `FULLY_FILLED` 約定済み, `CANCELED_UNFILLED` 取消済, `CANCELED_PARTIALLY_FILLED` 取消済(一部約定)
type | string | `limit`、`market`、`stop`、`stop_limit`、`take_profit`、`stop_loss`, `losscut`のうちいずれか
expire_at | number | 有効期限(UnixTimeのミリ秒)
triggered_at | number \| undefined | トリガー日時(UnixTimeのミリ秒)（type = `stop`, `stop_limit`, `take_profit`, `stop_loss` 時のみ）
post_only | boolean \| undefined | Post Onlyかどうか（type = `limit`時のみ）
user_cancelable | boolean | ユーザがキャンセル可能な注文かどうか
is_just_triggered | boolean | トリガーされたばかりかどうか

**レスポンスのフォーマット:**

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

#### 注文変更通知 spot_order

注文更新情報を受信します。

内容は `spot_order_new` と同一です。

手元でアクティブ注文を管理されている場合は、statusが `FULLY_FILLED`、 `CANCELED_UNFILLED`、 `CANCELED_PARTIALLY_FILLED` の通知を受け取った時、注文が約定、あるいはキャンセルされることを示しているため、手元の注文情報から削除してください。

**レスポンスのフォーマット:**

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

#### 注文無効通知 spot_order_invalidation

注文無効情報を受信します。

こちらの通知は弊社マッチングエンジン内での資産不足等の理由により、注文が無効となった場合に通知されます。
そのため、こちらの通知は頻繁に発生することはありません。
通常発生するような、注文の即時キャンセルは `spot_order_new`、 `spot_order` もしくはREST APIのエラーレスポンスで通知されます。

手元でアクティブ注文を管理されている場合は、手元の注文情報から通知された注文を削除してください。

Name | Type | Description
------------ | ------------ | ------------
order_id | number | 注文ID

**レスポンスのフォーマット:**

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

#### 約定情報通知 spot_trade

約定情報を受信します。

Name | Type | Description
------------ | ------------ | ------------
amount | string | 約定数量
executed_at | number | 約定日時（UnixTimeのミリ秒）
fee_amount_base | string | base手数料
fee_amount_quote | string | quote手数料
fee_occurred_amount_quote | string | quote発生手数料。信用取引では決済時に徴収。現物取引では即時徴収され`fee_amount_quote`と同値。
maker_taker | string | `maker` または `taker`
order_id | number | 注文ID
pair | string | 通貨ペア: [ペア一覧](pairs.md)
price | string | 価格
position_side | string \| null | `long` または `short`（信用取引の時のみ）
side | string | `buy` または `sell`
trade_id | number | 約定ID
type | string | `limit`、`market`、`stop`、`stop_limit`、`take_profit`、`stop_loss`, `losscut`のうちいずれか
profit_loss | string \| null | 実現損益
interest | string \| null | 利息

**レスポンスのフォーマット:**

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

#### 新規販売所注文情報通知 dealer_order_new

新規販売所注文情報を受信します。

Name | Type | Description
------------ | ------------ | ------------
order_id | string | 注文ID
asset | string | アセット名
side | string | `buy` または `sell`
price | string | 価格
amount | string | 注文数量
ordered_at | number | 注文日時

**レスポンスのフォーマット:**

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

#### 出金情報通知 withdrawal

出金情報を受信します。

Name | Type | Description
------------ | ------------ | ------------
uuid | string | 出金ID
network | string | ネットワーク名(暗号資産の時のみ): [ネットワーク一覧](networks.md)
asset | string | アセット名: [アセット一覧](assets.md)
account_uuid | string | 出金アカウントのID
amount | string | 出金数量
fee | string | 出金手数料
label | string | 出金先アドレスにつけたラベル(暗号資産の時のみ)
address | string | 出金先アドレス(暗号資産の時のみ)
txid | string \| null | 出金トランザクションID(暗号資産の時のみ)
bank_name | string | 出金先銀行(法定通貨の時のみ)
branch_name | string | 出金先銀行支店(法定通貨の時のみ)
account_type | string | 出金先口座種別(法定通貨の時のみ)
account_number | string | 出金先口座番号(法定通貨の時のみ)
account_owner | string | 出金先口座名義(法定通貨の時のみ)
status | string | `CONFIRMING`, `EXAMINING`, `SENDING`,  `DONE`, `REJECTED`, `CANCELED`, `CONFIRM_TIMEOUT`
requested_at | number | リクエスト日時UNIXタイムスタンプ(ミリ秒)

**レスポンスのフォーマット:**

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

#### 入金情報通知 deposit

入金情報を受信します。

Name | Type | Description
------------ | ------------ | ------------
uuid | string | 入金ID
asset | string | アセット名: [アセット一覧](assets.md)
amount | string | 入金数量
network | string \| undefined | ネットワーク名: [ネットワーク一覧](networks.md)(暗号通貨入金のみ)
address | string \| undefined | 入金address(暗号通貨入金のみ)
txid | string \| undefined | 入金トランザクションID(暗号通貨入金のみ)
found_at | number | 検知UNIXタイムスタンプ(ミリ秒)
confirmed_at | number | 承認(残高追加確定時)UNIXタイムスタンプ(ミリ秒、承認後のみ存在)
status | string | 入金状態: `FOUND`, `CONFIRMED`, `DONE`

**レスポンスのフォーマット:**

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

#### 建玉ポジション情報通知 margin_position_update

建玉ポジション情報を受信します。

Name | Type | Description
------------ | ------------ | ------------
pair | string | 通貨ペア: [ペア一覧](pairs.md)
position_side | string | `long` または `short`
average_price | string | 平均約定価格
open_amount | string | オープン数量
locked_amount | string | ロック数量
product | string | 平均約定価格 x オープン数量
unrealized_fee_amount | string | 未実現手数料
unrealized_interest_amount | string | 未実現利息

**レスポンスのフォーマット:**

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

#### 決済損金額・不足金額情報通知 margin_payable_update

決済損金額・不足金情報を受信します。

Name | Type | Description
------------ | ------------ | ------------
amount | string | 金額

**レスポンスのフォーマット:**

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

#### 督促情報通知 margin_notice_update

督促情報を受信します。

追証や不足金額が発生した際や、それらを一部差入・一部返済した際に配信されます。
また、それらを解消・完済した場合は全てのプロパティを null にしたものが配信されます。


Name | Type | Description
------------ | ------------ | ------------
what | string \| null | 何に対する催促か(marginCall or debt or settled)
occurred_at | number \| null | 発生日時
amount | string \| null | 金額
due_date_at | number \| null | 期限日時

**レスポンスのフォーマット:**

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

### ステップ5: PubNubに再接続する

プライベートストリームへの接続が切断された場合、以下の手順に従ってPubNubに再接続できます。

1. PubNubから切断されたことをstatusイベントから検知します。
2. PubNubに再接続します。

詳細な実装は[サンプルコード](#サンプルコード)を参照してください。

## サンプルコード

以下は、プライベートストリームに接続してユーザデータを受信する方法を示すサンプルコードです。

```javascript
// npm install pubnub
const PubNub = require('pubnub');
const crypto = require('crypto');

const API_KEY = '<your_api_key>';
const API_SECRET = '<your_api_secret>';

const PUBNUB_SUB_KEY = 'sub-c-ecebae8e-dd60-11e6-b6b1-02ee2ddab7fe';

const store = new Map();
// statusは0->3の方向にしか変化しない
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

    // statusが3=不活性の状態になった場合は1分後storeから削除する
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
   * 注意： いくつかのSDKでは、addListenerとは別にsubscriptionインスタンスにて個別のメッセージ受信メソッドを持っていますが、
   *       併用するとメッセージがどちらかにしか届かない可能性があるため、
   *       addListener内でstatus, messageの受信処理をまとめて行うことを推奨します。
   *
   * 以下は使用しないことを推奨します。
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

    // メッセージの順序は保証されないため、注文の順番が入れ替わっていたとしてもより状況が進んでいるもののみstoreに残るようにする必要がある
    // 注文messageが来たときにstoreを見て、なかったら追加、あったとしたら注文の状況が進んでいる時のみ更新する。
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