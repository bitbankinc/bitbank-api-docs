# Changelog

All notable changes to this project will be documented in this file. See [standard-version](https://github.com/conventional-changelog/standard-version) for commit guidelines.

## 1.1.0 (2021-06-08)


### Features

* **error:** add category and table of contents ([40ea517](https://github.com/bitbankinc/bitbank-api-docs/commit/40ea51742abf4f0fa855eebcbd785d07c09fe552))
* add post_only and expire_at ([#25](https://github.com/bitbankinc/bitbank-api-docs/issues/25)) ([0f0b1c4](https://github.com/bitbankinc/bitbank-api-docs/commit/0f0b1c4ed486b5fc414ed1de3da5efe47fbcc565))
* **rest-api:** add max count limit to GET /user/spot/trade_history ([#20](https://github.com/bitbankinc/bitbank-api-docs/issues/20)) ([e6a2027](https://github.com/bitbankinc/bitbank-api-docs/commit/e6a202781a611f76ec88cb24835b24696ad01275))


### Bug Fixes

* add open in ticker response ([#27](https://github.com/bitbankinc/bitbank-api-docs/issues/27)) ([655b175](https://github.com/bitbankinc/bitbank-api-docs/commit/655b175d58e1f71fa730fb8c1e4fc38c411da9ef))
* **public-api:** add missing 1month for candle-type of candlestick ([#21](https://github.com/bitbankinc/bitbank-api-docs/issues/21)) ([a6ea956](https://github.com/bitbankinc/bitbank-api-docs/commit/a6ea9569b3aba3f4833a19213168adecdf116a43))
* **rest-api:** style ([#26](https://github.com/bitbankinc/bitbank-api-docs/issues/26)) ([1076ed3](https://github.com/bitbankinc/bitbank-api-docs/commit/1076ed379074c2c7fa954ea9de5a4f1e58f0f6ca))
* typo in errors.md ([898cf1f](https://github.com/bitbankinc/bitbank-api-docs/commit/898cf1f7f4188e9627f980d78c9617028a0e32c4))
* typo in rest-api ([4e2d601](https://github.com/bitbankinc/bitbank-api-docs/commit/4e2d601f68651e2a89532ac1ad9d568bba200441))
* typo in rest-api ([16c14a6](https://github.com/bitbankinc/bitbank-api-docs/commit/16c14a6165f42f16cb7c798c5242a8ec3fbc0e03))

# CHANGELOG for Bitbank's API (2021-05-21)
---
## 2021-05-21
* Added a field to public-api and public-stream
  * open (response only)

---
## 2021-04-02
* fix style

---
## 2021-04-01
* Added some fields to rest-api
  * post_only (request and response)
  * expire_at (response only)
* Added some new error codes to `errors.md` and `errors_JP.md`

---
## 2021-03-17
* Added new pairs to
  * `public-api.md`.
  * `public-api_JP.md`.
  * `public-stream.md`.
  * `public-stream_JP.md`.
  * `rest-api.md`.
  * `rest-api_JP.md`.

---
## 2021-01-27
* Added new pairs to
  * `public-api.md`.
  * `public-api_JP.md`.
  * `public-stream.md`.
  * `public-stream_JP.md`.
  * `rest-api.md`.
  * `rest-api_JP.md`.

---
## 2020-12-25
* Added missing 1month for candle-type of candlestick to `public-api.md` and `public-api_JP.md`

---
## 2020-12-23
* Added `count` limitation of "Fetch trade history" endpoint to `rest-api.md` and `rest-api_JP.md` ([#19](https://github.com/bitbankinc/bitbank-api-docs/issues/19))

---
## 2020-11-20
* Added some missing error codes to `errors.md`

---
## 2020-09-16
* Added new pairs to
  * `public-api.md`.
  * `public-api_JP.md`.
  * `public-stream.md`.
  * `public-stream_JP.md`.
  * `rest-api.md`.
  * `rest-api_JP.md`.

---
## 2020-05-24
* Added new pairs to
  * `public-api.md`.
  * `public-api_JP.md`.
  * `public-stream.md`.
  * `public-stream_JP.md`.
  * `rest-api.md`.
  * `rest-api_JP.md`.

---
## 2020-02-17
* Fix ([#10](https://github.com/bitbankinc/bitbank-api-docs/issues/10))
* Added candlestick params additional info to `public-api.md`.

---
## 2019-10-24
* Remove `realtime-api.md`
  * Realtime API, PubNub was abolished.
  * Use public-stream instead.

---
## 2019-10-18
* Added new error codes to `errors.md`.
### Rest API
* New order status: REJECTED
  * The conditions to become REJECTED are as follows.
    * Market order (FOK) does not match the conditions and not fulfilled.
    * Limit order quantity is excessive for the order book.
* Changes to order behavior
  * The order execution condition for a market order is changed to FOK (Fill or Kill).
    * FOK (Fill or Kill) is an order execution condition that rejects (cancels)  order if all ordered quantities are not filled.
    * Order may become invalid if an excessive quantity is placed against the order book.
* New Exchange status: GET /spot/status
  * HALT
    * All order and cancel request suspended.
* New Pair statuses: GET /spot/pairs
  * is_enabled
  * stop_order
  * stop_order_and_cancel

---
## 2019-10-03
* Added wscat sample code to `public-stream.md`.

---
## 2019-10-02
* Added curl sample code to `rest-api.md`.

---
## 2019-10-01
* Added Realtime API documentation to `realtime-api.md`.

---
## 2019-09-27
* Added first version.
