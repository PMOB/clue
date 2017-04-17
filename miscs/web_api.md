Web APIの利用例
========

Web APIとは
-----------
Web上で提供されているアプリケーションを利用するためのインターフェースをWeb APIといいます。
あとHTTP APIとかいいます。 好きなように呼んでいいと思います。


利用例
========
APIの利用例として、YDN (Yahoo! Developer Network) のAPIから天気の情報を取得してみましょう。
対象のドキュメンテーションはこちらです。

https://developer.yahoo.com/weather/

このドキュメントから、

- リクエストのスキーマ
- レスポンスのスキーマ
- APIの利用条件

がわかります。

試しにcurlでクエリを飛ばしてデータを取得してみます。
東京の woeid は 1118370 だったので、ここからスキーマの `item` のなかの `condition` フィールド、つまり現在の天気情報を取得します。
Yahoo! Weatherのデータベースは `weather.forecast` ですから、呼び出すクエリは
`select itemcondition from weather.forecast where woeid=1118370`
となります。
ついでに温度がカ氏だとキモいのでセ氏に変えるため、`u='c'`を条件に付け加えます。

```sh
$ curl "https://query.yahooapis.com/v1/public/yql?" -d "q=select item.condition from weather.forecast where woeid=1118370 and u='c'" -d "format=json"
{"query":{"count":1,"created":"2017-02-03T05:26:47Z","lang":"en-US","results":{"channel":{"item":{"condition":{"code":"23","date":"Fri, 03 Feb 2017 01:00 PM JST","temp":"11","text":"Breezy"}}}}}}
```

無事取ってこれました。
このように、Web APIを利用するとWeb上のサービスをHTTPリクエストなどを通して利用できます。
