# みずほ銀行の為替データのダウンロードが失敗するようになったのを対応する

業務で1日に1回みずほ銀行のから[外国為替相場情報/ヒストリカルデータ](https://www.mizuhobank.co.jp/market/historical/index.html)日次の CSV ファイルを落としていたのだが、2026/03/18 からダウンロードが失敗するようになった.

確認したところ、悪さをしているのは Akamai のようだ.

```
$ curl -D - 'https://www.mizuhobank.co.jp/market/quote.csv'
HTTP/1.1 403 Forbidden
Server: AkamaiGHost
Mime-Version: 1.0
Content-Type: text/html
Content-Length: 402
Expires: Wed, 25 Mar 2026 02:01:02 GMT
Date: Wed, 25 Mar 2026 02:01:02 GMT
Connection: close
Access-Control-Allow-Methods: GET, HEAD, POST
Access-Control-Allow-Origin: https://search.www.mizuhobank.co.jp

<HTML><HEAD>
<TITLE>Access Denied</TITLE>
</HEAD><BODY>
<H1>Access Denied</H1>

You don't have permission to access "http&#58;&#47;&#47;www&#46;mizuhobank&#46;co&#46;jp&#47;market&#47;quote&#46;csv" on this server.<P>
Reference&#32;&#35;18&#46;5552cd17&#46;1774404062&#46;e3023083
<P>https&#58;&#47;&#47;errors&#46;edgesuite&#46;net&#47;18&#46;5552cd17&#46;1774404062&#46;e3023083</P>
</BODY>
</HTML>
```

https://errors.edgesuite.net でググると色々出てきて、うわあって感じ. とはいえ、ブラウザで普通にアクセスする分にはエラーにならないので、ブラウザっぽいリクエストにすれば多分動くんだろうなと試行錯誤して、最小で動くのはこんな感じだと分かった.

```
curl 'https://www.mizuhobank.co.jp/market/quote.csv' \
  -H 'User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:148.0) Gecko/20100101 Firefox/148.0' \
  -H 'Accept-Encoding: gzip' \
  -H 'Accept-Language: ja' \
  -H 'Accept: */*'
```

User-Agent がぱっとググって出てきた Internet Explorer 11 とか Chrome 94 では動かなくて、定期的に更新しないとダメなやつか!?と憂鬱ではあるのですが、一旦動いたので良しとした.
