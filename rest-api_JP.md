[English](rest-api.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Private REST API一覧](#private-rest-api%E4%B8%80%E8%A6%A7)
  - [API 概要](#api-%E6%A6%82%E8%A6%81)
  - [認証](#%E8%AA%8D%E8%A8%BC)
    - [ACCESS-TIME-WINDOW方式](#access-time-window%E6%96%B9%E5%BC%8F)
    - [ACCESS-NONCE方式](#access-nonce%E6%96%B9%E5%BC%8F)
    - [署名](#%E7%BD%B2%E5%90%8D)
      - [サンプル](#%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB)
        - [ACCESS-TIME-WINDOW](#access-time-window)
        - [ACCESS-NONCE](#access-nonce)
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
    - [信用取引情報](#%E4%BF%A1%E7%94%A8%E5%8F%96%E5%BC%95%E6%83%85%E5%A0%B1)
      - [信用取引ステータスを取得する](#%E4%BF%A1%E7%94%A8%E5%8F%96%E5%BC%95%E3%82%B9%E3%83%86%E3%83%BC%E3%82%BF%E3%82%B9%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
      - [建玉・追証・不足金額情報を取得する](#%E5%BB%BA%E7%8E%89%E3%83%BB%E8%BF%BD%E8%A8%BC%E3%83%BB%E4%B8%8D%E8%B6%B3%E9%87%91%E9%A1%8D%E6%83%85%E5%A0%B1%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
    - [約定履歴](#%E7%B4%84%E5%AE%9A%E5%B1%A5%E6%AD%B4)
      - [約定履歴を取得する](#%E7%B4%84%E5%AE%9A%E5%B1%A5%E6%AD%B4%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
    - [入金](#%E5%85%A5%E9%87%91)
      - [入金履歴を取得する](#%E5%85%A5%E9%87%91%E5%B1%A5%E6%AD%B4%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
      - [未反映入金を取得する](#%E6%9C%AA%E5%8F%8D%E6%98%A0%E5%85%A5%E9%87%91%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
      - [送付人を取得する](#%E9%80%81%E4%BB%98%E4%BA%BA%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
      - [未反映入金に送付人を登録する](#%E6%9C%AA%E5%8F%8D%E6%98%A0%E5%85%A5%E9%87%91%E3%81%AB%E9%80%81%E4%BB%98%E4%BA%BA%E3%82%92%E7%99%BB%E9%8C%B2%E3%81%99%E3%82%8B)
      - [未反映入金に送付人を一括登録する](#%E6%9C%AA%E5%8F%8D%E6%98%A0%E5%85%A5%E9%87%91%E3%81%AB%E9%80%81%E4%BB%98%E4%BA%BA%E3%82%92%E4%B8%80%E6%8B%AC%E7%99%BB%E9%8C%B2%E3%81%99%E3%82%8B)
    - [出金](#%E5%87%BA%E9%87%91)
      - [出金アカウントを取得する](#%E5%87%BA%E9%87%91%E3%82%A2%E3%82%AB%E3%82%A6%E3%83%B3%E3%83%88%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
      - [出金リクエストを行う](#%E5%87%BA%E9%87%91%E3%83%AA%E3%82%AF%E3%82%A8%E3%82%B9%E3%83%88%E3%82%92%E8%A1%8C%E3%81%86)
      - [出金履歴を取得する](#%E5%87%BA%E9%87%91%E5%B1%A5%E6%AD%B4%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
    - [取引所ステータス](#%E5%8F%96%E5%BC%95%E6%89%80%E3%82%B9%E3%83%86%E3%83%BC%E3%82%BF%E3%82%B9)
      - [取引所ステータスを取得する](#%E5%8F%96%E5%BC%95%E6%89%80%E3%82%B9%E3%83%86%E3%83%BC%E3%82%BF%E3%82%B9%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
    - [銘柄詳細](#%E9%8A%98%E6%9F%84%E8%A9%B3%E7%B4%B0)
      - [銘柄詳細一覧を取得する](#%E9%8A%98%E6%9F%84%E8%A9%B3%E7%B4%B0%E4%B8%80%E8%A6%A7%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)
    - [プライベートストリーム](#%E3%83%97%E3%83%A9%E3%82%A4%E3%83%99%E3%83%BC%E3%83%88%E3%82%B9%E3%83%88%E3%83%AA%E3%83%BC%E3%83%A0)
      - [プライベートストリームのチャンネルとトークンを取得する](#%E3%83%97%E3%83%A9%E3%82%A4%E3%83%99%E3%83%BC%E3%83%88%E3%82%B9%E3%83%88%E3%83%AA%E3%83%BC%E3%83%A0%E3%81%AE%E3%83%81%E3%83%A3%E3%83%B3%E3%83%8D%E3%83%AB%E3%81%A8%E3%83%88%E3%83%BC%E3%82%AF%E3%83%B3%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Private REST API一覧

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
- ACCESS-NONCE、ACCESS-TIME-WINDOWどちらも指定した場合はACCESS-TIME-WINDOW方式が優先されます。

### ACCESS-TIME-WINDOW方式

- ACCESS-KEY : APIキーページで取得したAPIキー。
- ACCESS-SIGNATURE : 以下に記述する署名を指定します。
- ACCESS-REQUEST-TIME : 整数値。通常は現在時刻のUNIXタイムスタンプ(ミリ秒)を使用してください。タイムゾーンはUTCであることにご注意ください。
- ACCESS-TIME-WINDOW : 整数値。リクエストが有効になるミリ秒数を指定できます。未指定の場合デフォルトで5000が適用されます。5000以下の小さな値を使用することを推奨します。最大値は60000を超えることはできません。ロジックは以下の通りです。

```typescript
if (ACCESS-REQUEST-TIME < (serverTime + 1000) && (serverTime - ACCESS-REQUEST-TIME) <= ACCESS-TIME-WINDOW) {
  // process request
} else {
  // reject request
}
```

### ACCESS-NONCE方式

- ACCESS-KEY : APIキーページで取得したAPIキー。
- ACCESS-NONCE : 整数値。リクエスト毎に数を増加させる必要があります。通常はUNIXタイムスタンプを使用してください。
- ACCESS-SIGNATURE : 以下に記述する署名を指定します。

### 署名

- 署名作成は、以下の文字列を `HMAC-SHA256` 形式でAPIシークレットキーを使って署名した結果となります。
  - ACCESS-TIME-WINDOW方式の場合
    - GETの場合: 「ACCESS-REQUEST-TIME、ACCESS-TIME-WINDOW、リクエストのパス、クエリパラメータ」 を連結させたもの
    - POSTの場合: 「ACCESS-REQUEST-TIME、ACCESS-TIME-WINDOW、リクエストボディのJson文字列」 を連結させたもの
  - ACCESS-NONCE方式の場合
    - GETの場合: 「ACCESS-NONCE、リクエストのパス、クエリパラメータ」 を連結させたもの
    - POSTの場合: 「ACCESS-NONCE、リクエストボディのJson文字列」 を連結させたもの

*※ GETのACCESS-SIGNATUREで使用する「リクエストのパス」には"/v1"も含める必要があります。*

*※ POSTではパラメータをJson文字列にしてリクエストボディに含める必要があります。*

#### サンプル

##### ACCESS-TIME-WINDOW

- GET: /v1/user/assetsのケース

```bash
export API_SECRET="hoge"
export ACCESS_REQUEST_TIME="1721121776490"
export ACCESS_TIME_WINDOW="1000"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_REQUEST_TIME$ACCESS_TIME_WINDOW/v1/user/assets" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"


echo $ACCESS_SIGNATURE
9ec5745960d05573c8fb047cdd9191bd0c6ede26f07700bb40ecf1a3920abae8
```

- POST系のエンドポイントのケース

```bash
export API_SECRET="hoge"
export ACCESS_REQUEST_TIME="1721121776490"
export ACCESS_TIME_WINDOW="1000"
export REQUEST_BODY='{"pair": "xrp_jpy", "price": "20", "amount": "1","side": "buy", "type": "limit"}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_REQUEST_TIME$ACCESS_TIME_WINDOW$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"


echo $ACCESS_SIGNATURE
7868665738ae3f8a796224e0413c1351ddd7ec2af121db12815c0a5b74b8764c
```

##### ACCESS-NONCE

- GET: /v1/user/assetsのケース

```bash
export API_SECRET="hoge"
export ACCESS_NONCE="1721121776490"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/assets" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"


echo $ACCESS_SIGNATURE
f957817b95c3af6cf5e2e9dfe1503ea8088f46879d4ab73051467fd7b94f1aba
```

- POST系のエンドポイントのケース

```bash
export API_SECRET="hoge"
export ACCESS_NONCE="1721121776490"
export REQUEST_BODY='{"pair": "xrp_jpy", "price": "20", "amount": "1","side": "buy", "type": "limit"}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"


echo $ACCESS_SIGNATURE
8ef83c2b991765b18c95aade7678471747c06890a23a453c76238345b5c86fb8
```

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
asset | string  | アセット名: [アセット一覧](assets.md)
free_amount | string | 利用可能な量
amount_precision | number | 精度
onhand_amount | string | 保有量
locked_amount | string | ロックされている量
withdrawing_amount | string | ロックされている量のうち出金中数量
withdrawal_fee | { min: string, max: string } or { under: string, over: string, threshold:string } for `jpy` | 出金手数料
stop_deposit | boolean | 入金ステータス（全ネットワークが入金停止 = `true`）
stop_withdrawal | boolean | 出金ステータス（全ネットワークが出金停止 = `true`）
network_list | { asset: string, network: string, stop_deposit: boolean, stop_withdrawal: boolean, withdrawal_fee: string } or undefined for `jpy` | ネットワーク一覧
collateral_ratio | string | 代用掛け目

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/assets" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

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
        "withdrawing_amount": "string",
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
        ],
        "collateral_ratio": "string"
      },
      {
        "asset": "jpy",
        "free_amount": "string",
        "amount_precision": 0,
        "onhand_amount": "string",
        "locked_amount": "string",
        "withdrawing_amount": "string",
        "withdrawal_fee": {
            "under": "string",
            "over": "string",
            "threshold": "string"
        },
        "stop_deposit": false,
        "stop_withdrawal": false,
        "collateral_ratio": "string"
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
order_id | number | order id
pair | string | 通貨ペア: [ペア一覧](pairs.md)
side | string | `buy` または `sell`
position_side | string \| undefined | `long` または `short`（信用取引の時のみ）
type | string | `limit`、`market`、`stop`、`stop_limit`、`take_profit`、`stop_loss`, `losscut`のうちいずれか
start_amount | string \| null | 注文時の数量
remaining_amount | string \| null | 未約定の数量
executed_amount| string | 約定済み数量
price | string \| undefined | 注文価格（type = `limit` または `stop_limit` 時のみ）
post_only | boolean \| undefined | Post Onlyかどうか（type = `limit`時のみ）
user_cancelable | boolean | ユーザがキャンセル可能な注文かどうか
average_price | string | 平均約定価格
ordered_at | number | 注文日時(UnixTimeのミリ秒)
expire_at | number \| null | 有効期限(UnixTimeのミリ秒)
triggered_at | number \| undefined | トリガー日時(UnixTimeのミリ秒)（type = `stop`, `stop_limit`, `take_profit`, `stop_loss` 時のみ）
trigger_price | string \| undefined | トリガー価格（type = `stop`, `stop_limit`, `take_profit`, `stop_loss` 時のみ）
status | string | 注文ステータス: `INACTIVE` 非アクティブ, `UNFILLED` 注文中, `PARTIALLY_FILLED` 注文中(一部約定), `FULLY_FILLED` 約定済み, `CANCELED_UNFILLED` 取消済, `CANCELED_PARTIALLY_FILLED` 取消済(一部約定)

**注意事項:**

* このAPIでは3ヶ月以上前の約定済またはキャンセル済注文を取得できません。（50009が返ります。）
  3ヶ月以上前の注文情報の取得にはお手数ですが[注文履歴の抽出ページ](https://app.bitbank.cc/account/data/orders/download)をご利用ください。

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/spot/order?pair=btc_jpy&order_id=1" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/spot/order?pair=btc_jpy\&order_id=1
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
    "position_side": "string",
    "type": "string",
    "start_amount": "string",
    "remaining_amount": "string",
    "executed_amount": "string",
    "price": "string",
    "post_only": false,
    "user_cancelable": true,
    "average_price": "string",
    "ordered_at": 0,
    "expire_at": 0,
    "triggered_at": 0,
    "trigger_price": "string",
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
amount | string | NO | 注文量。typeが `take_profit`、`stop_loss` 以外の場合は必須
price | string | NO | 価格
side | string | YES | `buy` または `sell`
position_side | string | NO | `long` または `short`
type | string | YES | `limit`、`market`、`stop`、`stop_limit`、`take_profit`、`stop_loss`, `losscut`のうちいずれか
post_only | boolean | NO | Post Onlyかどうか（type = `limit` 時のみ `true` を指定可能。デフォルト `false`）
trigger_price | string | NO | トリガー価格

**Response:**

Name | Type | Description
------------ | ------------ | ------------
order_id | number | order id
pair | string | 通貨ペア: [ペア一覧](pairs.md)
side | string | `buy` または `sell`
position_side | string \| undefined | `long` または `short`（信用取引の時のみ）
type | string | `limit`、`market`、`stop`、`stop_limit`、`take_profit`、`stop_loss`, `losscut`のうちいずれか
start_amount | string \| null | 注文時の数量
remaining_amount | string \| null | 未約定の数量
executed_amount| string | 約定済み数量
price | string \| undefined | 注文価格（type = `limit` または `stop_limit` 時のみ）
post_only | boolean \| undefined | Post Onlyかどうか（type = `limit`時のみ）
user_cancelable | boolean | ユーザがキャンセル可能な注文かどうか
average_price | string | 平均約定価格
ordered_at | number | 注文日時(UnixTimeのミリ秒)
expire_at | number \| null | 有効期限(UnixTimeのミリ秒)
trigger_price | string \| undefined | トリガー価格（type = `stop`, `stop_limit`, `take_profit`, `stop_loss` 時のみ）
status | string | 注文ステータス: `INACTIVE` 非アクティブ, `UNFILLED` 注文中, `PARTIALLY_FILLED` 注文中(一部約定), `FULLY_FILLED` 約定済み, `CANCELED_UNFILLED` 取消済, `CANCELED_PARTIALLY_FILLED` 取消済(一部約定)

**注意事項:**
- circuit_break_info.mode が `NONE` 以外の場合は成行注文を行うことができず、`70020`エラーが返ります。
- circuit_break_info.mode が `NONE` 以外の場合は `post_only` オプションは `false` として扱われます。

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export REQUEST_BODY='{"pair": "xrp_jpy", "price": "20", "amount": "1","side": "buy", "type": "limit"}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

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
    "position_side": "string",
    "type": "string",
    "start_amount": "string",
    "remaining_amount": "string",
    "executed_amount": "string",
    "price": "string",
    "post_only": false,
    "user_cancelable": true,
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
order_id | number | order id
pair | string | 通貨ペア: [ペア一覧](pairs.md)
side | string | `buy` または `sell`
position_side | string \| undefined | `long` または `short`（信用取引の時のみ）
type | string | `limit`、`market`、`stop`、`stop_limit`、`take_profit`、`stop_loss`, `losscut`のうちいずれか
start_amount | string \| null | 注文時の数量
remaining_amount | string \| null | 未約定の数量
executed_amount| string | 約定済み数量
price | string \| undefined | 注文価格（type = `limit` または `stop_limit` 時のみ）
post_only | boolean \| undefined | Post Onlyかどうか（type = `limit`時のみ）
user_cancelable | boolean | ユーザがキャンセル可能な注文かどうか
average_price | string | 平均約定価格
ordered_at | number | 注文日時(UnixTimeのミリ秒)
expire_at | number \| null | 有効期限(UnixTimeのミリ秒)
canceled_at | number \| undefined | キャンセル日時(UnixTimeのミリ秒)
triggered_at | number \| undefined | トリガー日時(UnixTimeのミリ秒)（type = `stop`, `stop_limit`, `take_profit`, `stop_loss` 時のみ）
trigger_price | string \| undefined | トリガー価格（type = `stop`, `stop_limit`, `take_profit`, `stop_loss` 時のみ）
status | string | 注文ステータス: `INACTIVE` 非アクティブ, `UNFILLED` 注文中, `PARTIALLY_FILLED` 注文中(一部約定), `FULLY_FILLED` 約定済み, `CANCELED_UNFILLED` 取消済, `CANCELED_PARTIALLY_FILLED` 取消済(一部約定)

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export REQUEST_BODY='{"pair": "xrp_jpy", "order_id": 1}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

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
    "user_cancelable": true,
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
order_ids | number[] | YES | 注文ID。最大30個まで指定可能

**Response:**

Name | Type | Description
------------ | ------------ | ------------
orders | Array | [注文をキャンセルする](#注文をキャンセルする)のレスポンスオブジェクトのリスト

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
        "user_cancelable": true,
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

**注意事項:**

* このAPIでは3ヶ月以上前の約定済またはキャンセル済注文を取得できません。（エラーとならず、結果に含まれません。）
  3ヶ月以上前の注文情報の取得にはお手数ですが[注文履歴の抽出ページ](https://app.bitbank.cc/account/data/orders/download)をご利用ください。

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export REQUEST_BODY='{"pair": "xrp_jpy", "order_ids": [1]}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' -H "Content-Type: application/json" -d ''"$REQUEST_BODY"'' https://api.bitbank.cc/v1/user/spot/orders_info
```

</p>
</details>

**Response:**

Name | Type | Description
------------ | ------------ | ------------
orders | Array | [注文情報を取得する](#注文情報を取得する)のレスポンスオブジェクトのリスト

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
        "position_side": "string",
        "type": "string",
        "start_amount": "string",
        "remaining_amount": "string",
        "executed_amount": "string",
        "price": "string",
        "post_only": false,
        "user_cancelable": true,
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
#### アクティブな注文を取得する

```txt
GET /user/spot/active_orders
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | NO | 通貨ペア: [ペア一覧](pairs.md)。注文IDを指定する場合pairの指定も必須
count | number | NO | 取得する注文数
from_id | number | NO | 取得開始注文ID
end_id | number | NO | 取得終了注文ID
since | number | NO | 開始UNIXタイムスタンプ
end | number | NO | 終了UNIXタイムスタンプ

**Response:**

Name | Type | Description
------------ | ------------ | ------------
orders | Array | [注文情報を取得する](#注文情報を取得する)のレスポンスオブジェクトのリスト

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/spot/active_orders?pair=btc_jpy" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

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
        "position_side": "string",
        "type": "string",
        "start_amount": "string",
        "remaining_amount": "string",
        "executed_amount": "string",
        "price": "string",
        "post_only": false,
        "user_cancelable": true,
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

### 信用取引情報

#### 信用取引ステータスを取得する

```txt
GET /user/margin/status
```

**Parameters:**
None

**Response:**

Name | Type | Description
---- | ---- | -----------
status | string | 口座状況ステータス: `NORMAL` 正常, `LOSSCUT` 強制決済中, `CALL` 追証発生中, `DEBT` 不足金発生中, `SETTLED` 債権売却済。未申込時も `NORMAL` を返却します。
total_margin_balance_percentage | string \| null | 保証金率%。小数以下2桁になるように切り捨てています。建玉が無い時は `null` です。
total_margin_balance | string | 受入保証金合計額。小数以下4桁になるように切り捨てています。
margin_position_profit_loss | string | 評価損益。小数以下4桁になるように-∞方向に丸めています。
unrealized_cost | string | 未収費用。小数以下4桁になるように+∞方向に丸めています。
total_margin_position_product | string | 建玉合計。小数以下4桁になるように切り捨てています。
open_margin_position_product | string | 建玉の保有分。小数以下4桁になるように切り捨てています。
open_margin_order_product | string | 建玉の新規発注中。小数以下4桁になるように切り捨てています。
total_position_maintenance_margin | string | 維持必要保証金額。小数以下4桁になるように切り上げています。
total_long_position_maintenance_margin | string | 維持必要保証金額のロング分。小数以下4桁になるように切り上げています。
total_short_position_maintenance_margin | string | 維持必要保証金額のショート分。小数以下4桁になるように切り上げています。
total_open_order_maintenance_margin | string | 約定時必要保証金額。小数以下4桁になるように切り上げています。
total_long_open_order_maintenance_margin | string | 約定時必要保証金額のロング分。小数以下4桁になるように切り上げています。
total_short_open_order_maintenance_margin | string | 約定時必要保証金額のショート分。小数以下4桁になるように切り上げています。
margin_call_percentage | string \| null | 追加保証金率%。小数以下0桁になるように切り上げています。建玉が無い時は `null` です。
losscut_percentage | string \| null | 強制決済率%。小数以下0桁になるように切り上げています。建玉が無い時は `null` です。
buy_credit | string | 買いのご利用可能枠
sell_credit | string | 売りのご利用可能枠
available_balances | {pair: string, long: string, short: string}[] | 新規建てご利用可能額

available_balances の各項目は以下の通りです:

Name | Type | Description
---- | ---- | -----------
pair | string | 信用ペア名
long | string | ロング新規建てご利用可能額。小数以下4桁になるように切り捨てています。
short | string | ショート新規建てご利用可能額。小数以下4桁になるように切り捨てています。

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/margin/status" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/margin/status
```

</p>
</details>

**レスポンスのフォーマット:**

```json
{
  "success": 1,
  "data": {
    "status": "NORMAL",
    "total_margin_balance_percentage": null,
    "total_margin_balance": "0.0000",
    "margin_position_profit_loss": "0.0000",
    "unrealized_cost": "0.0000",
    "total_margin_position_product": "0.0000",
    "open_margin_position_product": "0.0000",
    "open_margin_order_product": "0.0000",
    "total_position_maintenance_margin": "0.0000",
    "total_long_position_maintenance_margin": "0.0000",
    "total_short_position_maintenance_margin": "0.0000",
    "total_open_order_maintenance_margin": "0.0000",
    "total_long_open_order_maintenance_margin": "0.0000",
    "total_short_open_order_maintenance_margin": "0.0000",
    "margin_call_percentage": null,
    "losscut_percentage": null,
    "buy_credit": "0",
    "sell_credit": "0",
    "available_balances": [
      {
        "pair": "btc_jpy",
        "long": "0.0000",
        "short": "0.0000"
      },
    ]
  }
}
```

#### 建玉・追証・不足金額情報を取得する

```txt
GET /user/margin/positions
```

**Parameters:**
None

**Response:**

Name | Type | Description
------------ | ------------ | ------------
notice | { what: string \| null, occurred_at: number \| null, amount: string \| null, due_date_at: number \| null } | `追証` または `不足金` または `精算` に関する情報
payables | { amount: string } | 不足金額
positions | [{ pair: string, position_side: string, open_amount: string, product: string, average_price: string, unrealized_fee_amount: string, unrealized_interest_amount: string }] | 建玉情報
losscut_threshold | { individual: string, company: string } | 強制決済掛け目

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/margin/positions" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/margin/positions
```

</p>
</details>


**レスポンスのフォーマット:**

```json
{
  "success": 1,
  "data": {
    "notice": {
      "what": "string",
      "occurred_at": 0,
      "amount": "0",
      "due_date_at": 0
    },
    "payables": {
      "amount": "0"
    },
    "positions": [
      {
        "pair": "string",
        "position_side": "string",
        "open_amount": "0",
        "product": "0",
        "average_price": "0",
        "unrealized_fee_amount": "0",
        "unrealized_interest_amount": "0"
      }
    ],
    "losscut_threshold": {
      "individual": "0",
      "company": "0"
    }
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
pair | string | NO | 通貨ペア: [ペア一覧](pairs.md)。注文IDを指定する場合pairの指定も必須
count | number | NO | 取得する約定数(最大1000)
order_id | number | NO | 注文ID
since | number | NO | 開始UNIXタイムスタンプ(ミリ秒)
end | number | NO | 終了UNIXタイムスタンプ(ミリ秒)
order | string | NO | 約定時刻順序(`asc`: 昇順、`desc`: 降順、デフォルト`desc`)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
trade_id | number | trade id
pair | string | 通貨ペア: [ペア一覧](pairs.md)
order_id | number | 注文ID
side | string | `buy` または `sell`
position_side | string \| undefined | `long` または `short`（信用取引の時のみ）
type | string | `limit`、`market`、`stop`、`stop_limit`、`take_profit`、`stop_loss`, `losscut`のうちいずれか
amount | string | 注文量
price | string | 価格
maker_taker | string | `maker` または `taker`
fee_amount_base | string | base手数料
fee_amount_quote | string | quote手数料
fee_occurred_amount_quote | string | quote発生手数料。後ほど徴収される。現物取引ではfee_amount_quoteと同値。
profit_loss | string \| undefined | 実現損益
interest | string \| undefined | 利息
executed_at | number | 約定日時（UnixTimeのミリ秒）

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/spot/trade_history?pair=btc_jpy" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

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
        "position_side": "string",
        "type": "string",
        "amount": "string",
        "price": "string",
        "maker_taker": "string",
        "fee_amount_base": "string",
        "fee_amount_quote": "string",
        "profit_loss": "string",
        "interest": "string",
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
address | string | 入金address
asset | string | アセット名: [アセット一覧](assets.md)
network | string | ネットワーク名: [ネットワーク一覧](networks.md)
amount | number | 入金数量
txid | string \| null | 入金トランザクションID(暗号資産の時のみ)
status | string | 入金状態: `FOUND`, `CONFIRMED`, `DONE`
found_at | number | 検知UNIXタイムスタンプ(ミリ秒)
confirmed_at | number | 承認(残高追加確定時)UNIXタイムスタンプ(ミリ秒、承認後のみ存在)

**注意事項:**

* 現時点では入金履歴レスポンスには宛先タグ、メモおよび銀行口座情報が含まれていません。他システムの送金・送信との突合にはtxidをお使いください。

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/deposit_history?asset=btc" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

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

#### 未反映入金を取得する

```txt
GET /user/unconfirmed_deposits
```

**Parameters:**
None

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | 未反映入金 uuid
asset | string | アセット名: [アセット一覧](assets.md)
amount | string | 入金数量
network | string | ネットワーク名: [ネットワーク一覧](networks.md)
txid | string | 入金トランザクションID
created_at | number| 作成UNIXタイムスタンプ(ミリ秒)

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/unconfirmed_deposits" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

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

#### 送付人を取得する

```txt
GET /user/deposit_originators
```

**Parameters:**
None

**Response:**

Name | Type | Description
------------ | ------------ | ------------
uuid | string | 送付人のuuid
label | string | 送付人ラベル
deposit_type | string | 入金元: `WALLET` プライベートウォレット, `ELSE` その他
deposit_purpose | string \| null | 入金の目的
originator_status | string | 送付人ステータス: `SCREENING` 審査中, `CONFIRMED` 審査完了, `REJECTED` 否認, `DEPRECATED` 要修正
originator_type | string | 送付人種別: `OWN` 本人, `PERSON` 本人でない個人, `COMPANY` 本人でない法人
originator_last_name | string \| null | 姓
originator_first_name | string \| null | 名
originator_country | string \| null | 国
originator_prefecture | string \| null | 都市・省・県
originator_city | string \| null | 市区町村
originator_address | string \| null | 番地
originator_building | string \| null | 建物名
originator_company_name | string \| null | 法人名称
originator_company_type | string \| null | 法人格
originator_company_type_position | string \| null | 法人格（前後）
uuid | string | 実質的支配者のuuid
name | string | 実質的支配者
country | string | 実質的支配者の居住国
prefecture | string \| null | 実質的支配者の居住地域（都市・省・県）

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/deposit_originators" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

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
        "originator_preference": "string",
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

#### 未反映入金に送付人を登録する

```txt
POST /user/confirm_deposits
```

**Parameters (requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
deposits | object[] | YES | 未反映入金uuidと送付人uuidオブジェクトの配列。内容の詳細は以下。

**depositsの詳細**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
uuid | string | YES | 未反映入金 uuid
originator_uuid | string | YES | 送付人 uuid

**Request format:**

```json
{
  "deposits": [
    {
      "uuid": "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "originator_uuid": "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    },
    {
      "uuid": "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "originator_uuid": "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    },
    {
      …
    }
  ]
}
```

**Response:**
None

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export REQUEST_BODY='{"deposits": [{ "uuid": "___deposit uuid___", "originator_uuid": "___originator uuid___" }]}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

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

#### 未反映入金に送付人を一括登録する

```txt
POST /user/confirm_deposits_all
```

**Parameters (requestBody):**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
originator_uuid | string | YES | 送付人uuid

**Response:**
None

**Sample code:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export REQUEST_BODY='{"originator_uuid": "___originator uuid___"}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

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
network | string | ネットワーク名: [ネットワーク一覧](networks.md)
address | string | 出金先アドレス

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/withdrawal_account?asset=btc" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

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
        "network": "string",
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
amount | string | 出金数量
fee | string | 出金手数料
label | string | 出金先アドレスにつけたラベル(暗号資産の時のみ)
address | string | 出金先アドレス(暗号資産の時のみ)
network | string | ネットワーク名(暗号資産の時のみ): [ネットワーク一覧](networks.md)
destination_tag | number or string | 出金先宛先タグまたはメモ(タグまたはメモを指定した暗号資産の出金時のみ)
txid | string \| null | 出金トランザクションID(暗号資産の時のみ)
bank_name | string | 出金先銀行(法定通貨の時のみ)
branch_name | string | 出金先銀行支店(法定通貨の時のみ)
account_type | string | 出金先口座種別(法定通貨の時のみ)
account_number | string | 出金先口座番号(法定通貨の時のみ)
account_owner | string | 出金先口座名義(法定通貨の時のみ)
status | string | `CONFIRMING`, `EXAMINING`, `SENDING`,  `DONE`, `REJECTED`, `CANCELED`, `CONFIRM_TIMEOUT`
requested_at | number | リクエスト日時UNIXタイムスタンプ(ミリ秒)

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export REQUEST_BODY='{"asset": "xrp", "uuid": "___your uuid___", "amount": "1"}'
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE$REQUEST_BODY" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

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
uuid | string | 出金識別ID
asset | string | アセット名: [アセット一覧](assets.md)
account_uuid | string | 出金アカウントのID
amount | string | 出金数量
fee | string | 出金手数料
label | string | 出金先アドレスにつけたラベル(暗号資産の時のみ)
address | string | 出金先アドレス(暗号資産の時のみ)
network | string | ネットワーク名(暗号資産の時のみ): [ネットワーク一覧](networks.md)
destination_tag | number or string | 出金先宛先タグまたはメモ(タグまたはメモを指定した暗号資産の出金時のみ)
txid | string \| null | 出金トランザクションID(暗号資産の時のみ)
bank_name | string | 出金先銀行(法定通貨の時のみ)
branch_name | string | 出金先銀行支店(法定通貨の時のみ)
account_type | string | 出金先口座種別(法定通貨の時のみ)
account_number | string | 出金先口座番号(法定通貨の時のみ)
account_owner | string | 出金先口座名義(法定通貨の時のみ)
status | string | `CONFIRMING`, `EXAMINING`, `SENDING`,  `DONE`, `REJECTED`, `CANCELED`, `CONFIRM_TIMEOUT`
requested_at | number | リクエスト日時UNIXタイムスタンプ(ミリ秒)

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/withdrawal_history?asset=btc" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

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
margin_open_maker_fee_rate_quote | string \| null | 新規建てmaker手数料率(クオート資産)
margin_open_taker_fee_rate_quote | string \| null | 新規建てtaker手数料率(クオート資産)
margin_close_maker_fee_rate_quote | string \| null | 決済maker手数料率(クオート資産)
margin_close_taker_fee_rate_quote | string \| null | 決済maker手数料率(クオート資産)
margin_long_interest | string \| null | ロング利息率/日
margin_short_interest | string \| null | ショート利息率/日
margin_current_individual_ratio | string \| null | 現在の個人のリスク想定比率
margin_current_individual_until | number \| null | 現在の個人のリスク想定比率の適用終了日時（UnixTimeのミリ秒）
margin_current_company_ratio | string \| null | 現在の法人のリスク想定比率
margin_current_company_until | number \| null | 現在の法人のリスク想定比率の適用終了日時（UnixTimeのミリ秒）
margin_next_individual_ratio | string \| null | 次の個人のリスク想定比率
margin_next_individual_until | number \| null | 次の個人のリスク想定比率の適用終了日時（UnixTimeのミリ秒）
margin_next_company_ratio | string \| null | 次の法人のリスク想定比率
margin_next_company_until | number \| null | 次の法人のリスク想定比率の適用終了日時（UnixTimeのミリ秒）
unit_amount | string | 最小注文数量
limit_max_amount | string | 最大注文数量
market_max_amount | string | 成行注文時の最大数量
market_allowance_rate | string | 成行買注文時の余裕率
price_digits | number | 価格切り捨て対象桁数(0起点)
amount_digits | number | 数量切り捨て対象桁数(0起点)
is_enabled | boolean | 通貨ペアステータス(有効/無効)
stop_order | boolean | 注文停止ステータス
stop_order_and_cancel | boolean | 注文および注文キャンセル停止ステータス
stop_market_order | boolean | 成行注文停止ステータス
stop_stop_order | boolean | 逆指値(成行)注文停止ステータス
stop_stop_limit_order | boolean | 逆指値(指値)注文停止ステータス
stop_margin_long_order | boolean | ロング新規建て注文停止ステータス
stop_margin_short_order | boolean | ショート新規建て注文停止ステータス
stop_buy_order | boolean | 買い注文停止ステータス
stop_sell_order | boolean | 売り注文停止ステータス

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
        "margin_open_maker_fee_rate_quote": "string",
        "margin_open_taker_fee_rate_quote": "string",
        "margin_close_maker_fee_rate_quote": "string",
        "margin_close_taker_fee_rate_quote": "string",
        "margin_long_interest": "string",
        "margin_short_interest": "string",
        "margin_current_individual_ratio": "string",
        "margin_current_individual_until": 0,
        "margin_current_company_ratio": "string",
        "margin_current_company_until": 0,
        "margin_next_individual_ratio": "string",
        "margin_next_individual_until": 0,
        "margin_next_company_ratio": "string",
        "margin_next_company_until": 0,
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
        "stop_margin_long_order": false,
        "stop_margin_short_order": false,
        "stop_buy_order": false,
        "stop_sell_order": false
      }
    ]
  }
}
```

### プライベートストリーム

#### プライベートストリームのチャンネルとトークンを取得する

プライベートストリームのチャンネルとトークンを取得します。
チャンネルとトークンの使い方は[こちら](private-stream_JP.md)を参照してください。

pubnub_channelは、ユーザごとに異なるチャンネル名が割り当てられます。
pubnub_tokenには12時間の有効期限が設定されています。
pubnub_tokenの有効期限が切れると、PubNubとの接続が切断されるため、再度このAPIを呼び出して新しいpubnub_tokenを取得してください。

```txt
GET /user/subscribe
```

**Parameters:**
None

**Response:**

Name | Type | Description
------------ | ------------ | ------------
pubnub_channel | string | チャンネル
pubnub_token | string | トークン

**サンプルコード:**

<details>
<summary>Curl</summary>
<p>

```sh
export API_KEY=___your api key___
export API_SECRET=___your api secret___
export ACCESS_NONCE="$(date +%s)000"
export ACCESS_SIGNATURE="$(echo -n "$ACCESS_NONCE/v1/user/subscribe" | openssl dgst -sha256 -hmac "$API_SECRET" | awk '{print $NF}')"

curl -H 'ACCESS-KEY:'"$API_KEY"'' -H 'ACCESS-NONCE:'"$ACCESS_NONCE"'' -H 'ACCESS-SIGNATURE:'"$ACCESS_SIGNATURE"'' https://api.bitbank.cc/v1/user/subscribe
```

</p>
</details>


**レスポンスのフォーマット:**

```json
{
  "success": 1,
  "data": {
    "pubnub_channel": "string",
    "pubnub_token": "string"
  }
}
```
