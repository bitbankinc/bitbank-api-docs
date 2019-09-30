[English](realtime-api.md)

# Realtime API (2019-10-01) 

リアルタイムデータ配信には[Pubnub](https://www.pubnub.com/)を使用しています。以下のSubscribeKeyを利用することでデータの受信を行うことができます。

```
sub-c-e12e9174-dd60-11e6-806b-02ee2ddab7fe
```

Pubnubライブラリの使用方法に関しては [https://www.pubnub.com/docs](https://www.pubnub.com/docs) をご参照ください。

チャンネル名は以下の通りです。（アルトコインの場合は"btc_jpy"の部分を"xrp_jpy", "ltc_btc", "eth_btc", "mona_jpy"などに変更してください）

- ティッカー: ticker_btc_jpy
- 板情報: depth_btc_jpy
- 約定履歴: transactions_btc_jpy
- チャート: candlestick_btc_jpy