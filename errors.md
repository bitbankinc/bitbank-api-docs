<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Error codes for Bitbank (2024-08-22)](#error-codes-for-bitbank-2024-08-22)
  - [SYSTEM_ERROR](#system_error)
  - [AUTHENTICATION_ERROR](#authentication_error)
  - [REQUIRED_PARAMETER_ERROR](#required_parameter_error)
  - [INVALID_PARAMETER_ERROR](#invalid_parameter_error)
  - [DATA_ERROR](#data_error)
  - [VALUE_ERROR](#value_error)
  - [STOP_UPDATE_REQUEST_SYSTEM_STATUS](#stop_update_request_system_status)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

[日本語](errors_JP.md)

# Error codes for Bitbank (2024-08-22)

Here is the format of error JSON payload:

```json
{
  "success": 0,
  "data": {
    "code": 20003
  }
}
```

The following is the list of Bitbank's error codes.

## SYSTEM_ERROR

- `10000` Url not found.
- `10001` System error.
- `10002` Malformed request.
- `10003` System error.
- `10005` Timeout waiting for response.
- `10007` System maintenance.
- `10008` Server is busy. Retry later.
- `10009` You sent requests too frequently. Retry later with decreased requests.

## AUTHENTICATION_ERROR

- `20001` Authentication failed api authorization.
- `20002` Invalid ACCESS-KEY.
- `20003` ACCESS-KEY not found.
- `20004` ACCESS-NONCE not found.
- `20005` Invalid ACCESS-SIGNATURE.
- `20011` MFA failed.
- `20014` SMS verification failed.
- `20018` Please login. (This happens when you request API without `/v1/`.)
- `20023` Missing OTP code.
- `20024` Missing SMS code.
- `20025` Missing OTP and SMS code.
- `20026` MFA is temporarily locked because too many failures. Please retry after 60 seconds.
- `20033` ACCESS-REQUEST-TIME not found.
- `20034` Invalid time of ACCESS-REQUEST-TIME.
- `20035` No request was sent within ACCESS-TIME-WINDOW.
- `20036` ACCESS-REQUEST-TIME and ACCESS-NONCE not found.
- `20037` Invalid ACCESS-REQUEST-TIME.
- `20038` Invalid ACCESS-TIME-WINDOW.
- `20039` Invalid ACCESS-NONCE.

## REQUIRED_PARAMETER_ERROR

- `30001` Missing order quantity.
- `30006` Missing order id.
- `30007` Missing order id array.
- `30009` Missing asset.
- `30012` Missing order price.
- `30013` Missing side.
- `30015` Missing order type.
- `30016` Missing asset.
- `30019` Missing uuid.
- `30039` Missing withdraw amount.
- `30101` Missing trigger price.
- `30103` Missing withdrawal type.
- `30104` Missing withdrawal name.
- `30105` Missing VASP.
- `30106` Missing beneficiary type.
- `30107` Missing beneficiary last name.
- `30108` Missing beneficiary first name.
- `30109` Missing beneficiary last kana.
- `30110` Missing beneficiary first kana.
- `30111` Missing beneficiary company name.
- `30112` Missing beneficiary company kana.
- `30113` Missing beneficiary company type.
- `30114` Missing beneficiary company type position.
- `30115` Missing uploaded documents.
- `30116` Missing withdrawal purpose.
- `30117` Missing beneficiary country.
- `30118` Missing beneficiary zip code.
- `30119` Missing beneficiary prefecture.
- `30120` Missing beneficiary city.
- `30121` Missing beneficiary address.
- `30122` Missing beneficiary building.
- `30123` Missing extraction request category.

## INVALID_PARAMETER_ERROR

- `40001` Invalid order quantity.
- `40006` Invalid count.
- `40007` Invalid end param.
- `40008` Invalid end_id.
- `40009` Invalid from_id.
- `40013` Invalid order id.
- `40014` Invalid order id array.
- `40015` Too many orders are specified.
- `40017` Invalid asset.
- `40020` Invalid order price.
- `40021` Invalid order side.
- `40022` Invalid trading start time.
- `40024` Invalid order type.
- `40025` Invalid asset.
- `40028` Invalid uuid.
- `40048` Invalid withdraw amount.
- `40112` Invalid trigger price.
- `40113` Invalid post_only.
- `40114` post_only can not be specified with such order type.
- `40116` Invalid withdrawal type.
- `40117` Invalid withdrawal name.
- `40118` Invalid VASP.
- `40119` Invalid beneficiary type.
- `40120` Invalid beneficiary last name.
- `40121` Invalid beneficiary first name.
- `40122` Invalid beneficiary last kana.
- `40123` Invalid beneficiary first kana.
- `40124` Invalid beneficiary company name.
- `40125` Invalid beneficiary company kana.
- `40126` Invalid beneficiary company type.
- `40127` Invalid beneficiary company type position.
- `40152` Invalid originator label.
- `40153` Invalid originator last name.
- `40154` Invalid originator first name.
- `40155` Invalid originator company name.
- `40156` Invalid originator prefecture.
- `40157` Invalid originator city.
- `40158` Invalid originator address.
- `40159` Invalid originator building.
- `40160` Invalid originator substantial controller name.
- `40163` Invalid beneficiary substantial controller name.

## DATA_ERROR

- `50003` Account is restricted.
- `50004` Account is provisional.
- `50005` Account is blocked.
- `50006` Account is blocked.
- `50008` Identity verification is not finished.
- `50009` Order not found.
- `50010` Order can not be canceled.
- `50011` Api not found.
- `50026` Order has already been canceled.
- `50027` Order has already been executed.
- `50033` Withdrawals to this address require additional entries.
- `50034` VASP not found.
- `50035` Company information is not registerd.
- `50037` We are temporarily restricting withdrawals while we verify your last deposit. Please try again in a few minutes.
- `50038` Cannot withdraw to chosen VASP service.
- `50043` Originator already registered.
- `50044` Originator not found.
- `50045` Deposit not found.
- `50046` Cannot edit beneficiary under review.
- `50047` Cannot edit disabled beneficiary.
- `50048` Cannot withdraw to beneficiary under review.
- `50049` Beneficiary requires additional entries.
- `50050` Cannot withdraw to chosen beneficiary.
- `50051` Cannot confirm deposit with originator under review.
- `50052` Originator requires additional entries.
- `50053` Cannot edit originator under review.
- `50054` Cannot withdraw because the information registration for unreflected deposits has not been completed.

## VALUE_ERROR

- `60001` Insufficient amount.
- `60002` Market buy order quantity has exceeded the upper limit.
- `60003` Order quantity has exceeded the limit.
- `60004` Order quantity has exceeded the lower threshold.
- `60005` Order price has exceeded the upper limit.
- `60006` Order price has exceeded the lower limit.
- `60011` Too many Simultaneous orders, current limit is 30.
- `60016` Trigger price has exceeded the upper limit.
- `60017` Withdrawal amount has exceeded the upper limit.

## STOP_UPDATE_REQUEST_SYSTEM_STATUS

- `70001` System error.
- `70002` System error.
- `70003` System error.
- `70004` Order is restricted during suspension of transactions.
- `70005` Buy ​​order has been temporarily restricted.
- `70006` Sell ​​order has been temporarily restricted.
- `70009` Market order has been temporarily restricted. Please use limit order instead.
- `70010` Minimum Order Quantity is increased temporarily.
- `70011` System is busy. Please try again.
- `70012` System error.
- `70013` Order and cancel has been temporarily restricted.
- `70014` Withdraw and cancel request has been temporarily restricted.
- `70015` Lending and cancel request has been temporarily restricted.
- `70016` Lending and cancel request has been restricted.
- `70017` Orders on pair have been suspended.
- `70018` Order and cancel on pair have been suspended.
- `70019` Order cancel request is in process.
- `70020` Market order has been temporarily restricted.
- `70021` Limit order price is over the threshold.
- `70022` Stop limit order has been temporarily restricted.
- `70023` Stop order has been temporarily restricted.
