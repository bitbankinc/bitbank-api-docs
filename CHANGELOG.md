# CHANGELOG for Bitbank's API (2020-02-17)
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
