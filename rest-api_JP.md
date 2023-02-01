[English](rest-api.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Private REST API一覧 (2022-04-26)](#private-rest-api%E4%B8%80%E8%A6%A7-2022-04-26)
  - [API 概要](#api-%E6%A6%82%E8%A6%81)
  - [認証](#%E8%AA%8D%E8%A8%BC)
  - [レートリミット](#%E3%83%AC%E3%83%BC%E3%83%88%E3%83%AA%E3%83%9F%E3%83%83%E3%83%88)
  - [エンドポイント一覧](#%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88%E4%B8%80%E8%A6%A7)
    - [アセット](#%E3%82%A2%E3%82%BB%E3%83%83%E3%83%88)
      - [アセット一覧を返す](#%E3%82%A2%E3%82%BB%E3%83%83%E3%83%88%E4%B8%80%E8%A6%A7%E3%82%92%E8%BF%94%E3%81%99)
    - [注文情報](#%E6%B3%A8%E6%96%87%E6%83%85%E5%A0%B1)
      - [注文情報を取得する](#%E6%B3%A8%E6%96%87%E6%83%85%E5%A0%B1%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
      - [新規注文を行う](#%E6%96%B0%E8%A6%8F%E6%B3%A8%E6%96%87%E3%82%92%E8%A1%8C%E3%81%86)
      - [注文をキャンセルする](#%E6%B3%A8%E6%96%87%E3%82%92%E3%82%AD%E3%83%A3%E3%83%B3%E3%82%BB%E3%83%AB%E3%81%99%E3%82%8B)
      - [注文をキャンセルする(複数)](#%E6%B3%A8%E6%96%87%E3%82%92%E3%82%AD%E3%83%A3%E3%83%B3%E3%82%BB%E3%83%AB%E3%81%99%E3%82%8B%E8%A4%87%E6%95%B0)
      - [注文情報を取得する(複数)](#%E6%B3%A8%E6%96%87%E6%83%85%E5%A0%B1%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B%E8%A4%87%E6%95%B0)
      - [アクティブな注文を取得する](#%E3%82%A2%E3%82%AF%E3%83%86%E3%82%A3%E3%83%96%E3%81%AA%E6%B3%A8%E6%96%87%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
    - [約定履歴](#%E7%B4%84%E5%AE%9A%E5%B1%A5%E6%AD%B4)
      - [約定履歴を取得する](#%E7%B4%84%E5%AE%9A%E5%B1%A5%E6%AD%B4%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
    - [入金](#%E5%85%A5%E9%87%91)
      - [入金履歴を取得する](#%E5%85%A5%E9%87%91%E5%B1%A5%E6%AD%B4%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
    - [出金](#%E5%87%BA%E9%87%91)
      - [出金アカウントを取得する](#%E5%87%BA%E9%87%91%E3%82%A2%E3%82%AB%E3%82%A6%E3%83%B3%E3%83%88%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
      - [出金リクエストを行う](#%E5%87%BA%E9%87%91%E3%83%AA%E3%82%AF%E3%82%A8%E3%82%B9%E3%83%88%E3%82%92%E8%A1%8C%E3%81%86)
      - [出金履歴を取得する](#%E5%87%BA%E9%87%91%E5%B1%A5%E6%AD%B4%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
    - [取引所ステータス](#%E5%8F%96%E5%BC%95%E6%89%80%E3%82%B9%E3%83%86%E3%83%BC%E3%82%BF%E3%82%B9)
      - [取引所ステータスを取得する](#%E5%8F%96%E5%BC%95%E6%89%80%E3%82%B9%E3%83%86%E3%83%BC%E3%82%BF%E3%82%B9%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
    - [銘柄詳細](#%E9%8A%98%E6%9F%84%E8%A9%B3%E7%B4%B0)
      - [銘柄詳細一覧を取得する](#%E9%8A%98%E6%9F%84%E8%A9%B3%E7%B4%B0%E4%B8%80%E8%A6%A7%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Private REST API一覧 (2022-04-26)

## API 概要

- エンドポイント URL: **[https://api.bitbank.cc/v1](https://api.bitbank.cc/v1)**
- リクエストに不正がある場合、以下のようなエラーレスポンスを返します。

```json
{
  "success": 0,
  "data": {
    "code": 20003
  }
}
```

- エラーコードの一覧は [errors_JP.md](errors_JP.md) を確認してください。
- `GET`エンドポイントの場合、パラメータをクエリー文字列として送信してください。
- `POST`エンドポイントの場合、リクエストヘッダ`Content-Type`：`application/json`で、リクエストボディにデータを記述してください。

## 認証

- パブリックAPIは認証情報無しでアクセスできます。プライベートAPIでは以下の情報をHTTPリクエストヘッダーに付与してリクエストを行う必要があります。
  - ACCESS-KEY : APIキーページで取得したAPIキー。
  - ACCESS-NONCE : 整数値。リクエスト毎に数を増加させる必要があります。通常はUNIXタイムスタンプを使用してください。
  - ACCESS-SIGNATURE : 以下に記述する署名を指定します。

- 署名作成は、以下の文字列を `HMAC-SHA256` 形式でAPIシークレットキーを使って署名した結果となります。
  - GETの場合: 「ACCESS-NONCE、リクエストのパス、クエリパラメータ」 を連結させたもの
  - POSTの場合: 「ACCESS-NONCE、リクエストボディのJson文字列」 を連結させたもの

*※ GETのACCESS-SIGNATUREで使用する「リクエストのパス」には"/v1"も含める必要があります。*

*※ POSTではパラメータをJson文字列にしてリクエストボディに含める必要があります。*

## レートリミット

- ユーザごと、更新系・取得系それぞれ、1秒ごとにREST API呼び出しの頻度に制限を設けています。
  - 更新系とは新規注文、注文キャンセル、出金リクエストのことを指します。
  - 取得系はそれ以外を指します。
- マッチングエンジンの過負荷を防ぐため、システム全体においてもREST API呼び出しに制限を設けています。
  - ただしこの制限はほとんどの場合において抵触しないくらい高く設定されています。
- レートリミットに抵触した場合、HTTP 429エラーを返します。429エラーがよく返却される場合はAPI呼び出しの頻度を減らしてください。
- 頻度制限について基本的に取得系は 10回/秒 、更新系は 6回/秒 としています。
  - 取引高等の実績に応じて引き上げが可能な場合があります。
  - 必要であれば[こちらのフォーム](https://support.bitbank.cc/hc/ja/requests/new?ticket_form_id=13179074196633)より送信ください。
    - 「問合せ内容」は「レートリミットの制限緩和について」をお選びください。

## エンドポイント一覧

### アセット

#### アセット一覧を返す

```txt
GET /user/assets
```

**Parameters:**
None

**Response:**

Name | Type | Description
------------ | ------------ | ------------
asset | string | アセット名: [アセット一覧](assets.md)
free_amount | string | 利用可能な量
amount_precision | number | 精度
onhand_amount | string | 保有量
locked_amount | string | ロックされている量
withdrawal_fee | string or { under: string, over: string, threshold:string } for `jpy` | 出金手数料
stop_deposit | boolean | 入金ステータス
stop_withdrawal | boolean | 出金ステータス

**サンプルコード:**

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


**レスポンスのフォーマット:**

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

### 注文情報

#### 注文情報を取得する

```txt
GET /user/spot/order
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)
order_id | number | YES | 取引ID

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | 取引ID
pair | string | 通貨ペア: [ペア一覧](pairs.md)
side | string | `buy` または `sell`
type | string | `limit` または `market` または `stop` または `stop_limit`
start_amount | string | 注文時の数量
remaining_amount | string | 未約定の数量
executed_amount| string | 約定済み数量
price | string |注文価格
post_only | boolean | Post Onlyかどうか（type = `limit`時のみ）
average_price | string | 平均約定価格
ordered_at | number | 注文日時(UnixTimeのミリ秒)
expire_at | number or null | 有効期限(UnixTimeのミリ秒)
triggered_at | number or null | トリガー日時(UnixTimeのミリ秒)
trigger_price | string | トリガー価格
status | string | 注文ステータス: `INACTIVE` 非アクティブ, `UNFILLED` 注文中, `PARTIALLY_FILLED` 注文中(一部約定), `FULLY_FILLED` 約定済み, `CANCELED_UNFILLED` 取消済, `CANCELED_PARTIALLY_FILLED` 取消済(一部約定)


**レスポンスのフォーマット:**

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
    "triger_price": "string",
    "status": "string"
  }
}
```

#### 新規注文を行う

```txt
POST /user/spot/order
```

**Parameters(requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)
amount | string | YES | 注文量
price | string | NO | 価格
side | string | YES  | `buy` または `sell`
type | string | YES | `limit` または `market` または `stop` または `stop_limit`
post_only | boolean | NO | Post Onlyかどうか（type = `limit` 時のみ指定可能。デフォルト `false`）
trigger_price | string | NO | トリガー価格

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | 取引ID
pair | string | 通貨ペア: [ペア一覧](pairs.md)
side | string | `buy` または `sell`
type | string | `limit` または `market` または `stop` または `stop_limit`
start_amount | string | 注文時の数量
remaining_amount | string | 未約定の数量
executed_amount| string | 約定済み数量
price | string | 注文価格
post_only | boolean | Post Onlyかどうか（type = `limit`時のみ）
average_price | string | 平均約定価格
ordered_at | number | 注文日時(UnixTimeのミリ秒)
expire_at | number | 有効期限(UnixTimeのミリ秒)
trigger_price | string | トリガー価格
status | string | 注文ステータス: `INACTIVE` 非アクティブ, `UNFILLED` 注文中, `PARTIALLY_FILLED` 注文中(一部約定), `FULLY_FILLED` 約定済み, `CANCELED_UNFILLED` 取消済, `CANCELED_PARTIALLY_FILLED` 取消済(一部約定)

**サンプルコード:**

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


**レスポンスのフォーマット:**

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

#### 注文をキャンセルする

```txt
POST /user/spot/cancel_order
```

**Parameters(requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)
order_id | number | YES | 注文ID

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | 注文ID
pair | string | 通貨ペア: [ペア一覧](pairs.md)
side | string | `buy` または `sell`
type | string | `limit` または `market` または `stop` または `stop_limit`
start_amount | string | 注文時の数量
remaining_amount | string | 未約定の数量
executed_amount| string | 約定済み数量
price | string | 注文価格
post_only | boolean | Post Onlyかどうか（type = `limit`時のみ）
average_price | string | 平均約定価格
ordered_at | number | 注文日時(UnixTimeのミリ秒)
expire_at | number | 有効期限(UnixTimeのミリ秒)
canceled_at | number | キャンセル日時(UnixTimeのミリ秒)
triggered_at | number or null | トリガー日時(UnixTimeのミリ秒)
trigger_price | string | トリガー価格
status | string | 注文ステータス: `INACTIVE` 非アクティブ, `UNFILLED` 注文中, `PARTIALLY_FILLED` 注文中(一部約定), `FULLY_FILLED` 約定済み, `CANCELED_UNFILLED` 取消済, `CANCELED_PARTIALLY_FILLED` 取消済(一部約定)

**サンプルコード:**

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


**レスポンスのフォーマット:**

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

#### 注文をキャンセルする(複数)

```txt
POST /user/spot/cancel_orders
```

**Parameters(requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)
order_ids | number[] | YES | 注文ID

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

#### 注文情報を取得する(複数)

```txt
POST /user/spot/orders_info
```

*POSTメソッド注意*

**Parameters (requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)
order_ids | number[] | YES | 注文ID

**サンプルコード:**

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


**レスポンスのフォーマット:**

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

#### アクティブな注文を取得する

```txt
GET /user/spot/active_orders
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)
count | number | NO | 取得する注文数
from_id | number | NO | 取得開始注文ID
end_id | number | NO | 取得終了注文ID
since | number | NO | 開始UNIXタイムスタンプ
end | number | NO | 終了UNIXタイムスタンプ

**Response:**

Name | Type | Description
------------ | ------------ | ------------
pair | string | 通貨ペア: [ペア一覧](pairs.md)
side | string | `buy` または `sell`
type | string | `limit` または `market` または `stop` または `stop_limit`
start_amount | string | 注文時の数量
remaining_amount | string | 未約定の数量
executed_amount| string | 約定済み数量
price | string | 注文価格
post_only | boolean | Post Onlyかどうか（type = `limit`時のみ）
average_price | string | 平均約定価格
ordered_at | number | 注文日時(UnixTimeのミリ秒)
expire_at | number | 有効期限(UnixTimeのミリ秒)
triggered_at | number or null | トリガー日時(UnixTimeのミリ秒)
trigger_price | string | トリガー価格
status | string | 注文ステータス: `INACTIVE` 非アクティブ, `UNFILLED` 注文中, `PARTIALLY_FILLED` 注文中(一部約定), `FULLY_FILLED` 約定済み, `CANCELED_UNFILLED` 取消済, `CANCELED_PARTIALLY_FILLED` 取消済(一部約定)

**サンプルコード:**

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


**レスポンスのフォーマット:**

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

### 約定履歴

#### 約定履歴を取得する

```txt
GET /user/spot/trade_history
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | 通貨ペア: [ペア一覧](pairs.md)
count | number | NO | 取得する約定数(最大1000)
order_id | number | NO | 注文ID
since | number | NO | 開始UNIXタイムスタンプ
end | number | NO | 終了UNIXタイムスタンプ
order | string | NO | 約定時刻順序(`asc`: 昇順、`desc`: 降順、デフォルト`desc`)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
trade_id | number | trade id
pair | string | 通貨ペア: [ペア一覧](pairs.md)
order_id | number | 注文ID
side | string | `buy` または `sell`
type | string | `limit` または `market` または `stop` または `stop_limit`
amount | string | 注文量
price | string | 価格
maker_taker | string | `maker` または `taker`
fee_amount_base | string | base手数料
fee_amount_quote | string | quote手数料
executed_at | number | 約定日時（UnixTimeのミリ秒）

**サンプルコード:**

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


**レスポンスのフォーマット:**

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

### 入金

#### 入金履歴を取得する

```txt
GET /user/deposit_history
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
asset | string | YES | アセット名: [アセット一覧](assets.md)
count | number | NO | 取得する履歴数(最大100)
since | number | NO | 開始UNIXタイムスタンプ(ミリ秒)
end | number | NO | 終了UNIXタイムスタンプ(ミリ秒)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | 入金識別uuid
asset | string | アセット名: [アセット一覧](assets.md)
amount | number | 入金数量
txid | string | 入金トランザクションID(暗号資産の時のみ)
status | string | 入金状態: `FOUND`, `CONFIRMED`, `DONE`
found_at | number| 検知UNIXタイムスタンプ(ミリ秒)
confirmed_at | number | 承認(残高追加確定時)UNIXタイムスタンプ(ミリ秒、承認後のみ存在)


**注意事項:**

* 現時点では入金履歴レスポンスにはアドレス、宛先タグ、メモおよび銀行口座情報が含まれていません。他システムの送金・送信との突合にはtxidをお使いください。

**サンプルコード:**

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


**レスポンスのフォーマット:**

```json
{
  "success": 1,
  "data": {
    "deposits": [
      {
        "uuid": "string",
        "asset": "string",
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

### 出金

#### 出金アカウントを取得する

```txt
GET /user/withdrawal_account
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
asset | string | YES | アセット名: [アセット一覧](assets.md)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | 出金アカウントのID
label | string | ラベル
address | string | 出金先アドレス

**サンプルコード:**

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


**レスポンスのフォーマット:**

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

#### 出金リクエストを行う

```txt
POST /user/request_withdrawal
```

**Parameters (requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
asset | string | YES | アセット名: [アセット一覧](assets.md)
uuid | string | YES | 出金アカウントのuuid
amount | string | YES | 出金数量
otp_token | string | NO | 二段階認証トークン(設定している場合、otp_tokenかsms_tokenのどちらか一方を指定)
sms_token | string | NO | SMS認証トークン

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | 出金識別ID
asset | string | アセット名: [アセット一覧](assets.md)
account_uuid | string | 出金アカウントのID
amount | number | 出金数量
fee | number | 出金手数料
label | string | ラベル
address | string | 出金先アドレス
txid | string | 出金トランザクションID
status | string | ステータス: `CONFIRMING`, `EXAMINING`, `SENDING`,  `DONE`, `REJECTED`, `CANCELED`, `CONFIRM_TIMEOUT`
requested_at | number | リクエスト日時(UnixTimeのミリ秒)


**サンプルコード:**

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



**レスポンスのフォーマット:**

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

#### 出金履歴を取得する

```txt
GET /user/withdrawal_history
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
asset | string | YES | アセット名: [アセット一覧](assets.md)
count | number | NO | 取得する履歴数(最大100)
since | number | NO | 開始UNIXタイムスタンプ(ミリ秒)
end | number | NO | 終了UNIXタイムスタンプ(ミリ秒)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | 出金識別uuid
asset | string | アセット名: [アセット一覧](assets.md)
account_uuid | string | 出金アカウントのID
amount | number | 出金数量
fee | number | 出金手数料
label | string | 出金先アドレスにつけたラベル(暗号資産の時のみ)
address | string | 出金先アドレス(暗号資産の時のみ)
destination_tag | number or string | 出金先宛先タグまたはメモ(タグまたはメモを指定した暗号資産の出金時のみ)
txid | string or null | 出金トランザクションID(暗号資産の時のみ)
bank_name | string | 出金先銀行(法定通貨の時のみ)
branch_name | string | 出金先銀行支店(法定通貨の時のみ)
account_type | string | 出金先口座種別(法定通貨の時のみ)
account_number | string | 出金先口座番号(法定通貨の時のみ)
account_owner | string | 出金先口座名義(法定通貨の時のみ)
status | string | ステータス: `CONFIRMING`, `EXAMINING`, `SENDING`,  `DONE`, `REJECTED`, `CANCELED`, `CONFIRM_TIMEOUT`
requested_at | number| リクエスト日時UNIXタイムスタンプ(ミリ秒)


**サンプルコード:**

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


**レスポンスのフォーマット:**

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

### 取引所ステータス

#### 取引所ステータスを取得する

*当該APIは認証不要となります。*

```txt
GET /spot/status
```

**Parameters:**
None

**Response:**

Name | Type | Description
------------ | ------------ | ------------
pair | string | 通貨ペア: [ペア一覧](pairs.md)
status | string | 取引所ステータス: `NORMAL`, `BUSY`, `VERY_BUSY`, `HALT`
min_amount| string | 取引所ステータスに応じた最小注文数量（負荷が高いほど大きくなります）


**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
curl https://api.bitbank.cc/v1/spot/status
```

</p>
</details>


**レスポンスのフォーマット:**

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

### 銘柄詳細

#### 銘柄詳細一覧を取得する

*当該APIは認証不要となります。*

```txt
GET /spot/pairs
```

**Parameters:**
None

**Response:**

Name | Type | Description
------------ | ------------ | ------------
name | string | 通貨ペア: [ペア一覧](pairs.md)
base_asset | string | 原資産
quote_asset | string | クオート資産
maker_fee_rate_base | string | メイカー手数料率(原資産)
taker_fee_rate_base | string | テイカー手数料率(原資産)
maker_fee_rate_quote | string | メイカー手数料率(クオート資産)
taker_fee_rate_quote | string | テイカー手数料率(クオート資産)
unit_amount| string | 最小注文数量
limit_max_amount | string | 最大注文数量
market_max_amount | string | 成行注文時の最大数量
market_allowance_rate | string | 成行買注文時の余裕率
price_digits | number | 価格切り捨て対象桁数(0起点)
amount_digits | number | 数量切り捨て対象桁数(0起点)
is_enabled | boolean | 通貨ペアステータス(有効/無効)
stop_order | boolean | 注文停止ステータス
stop_order_and_cancel | boolean | 注文および注文キャンセル停止ステータス


**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
curl https://api.bitbank.cc/v1/spot/pairs
```

</p>
</details>



**レスポンスのフォーマット:**

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
