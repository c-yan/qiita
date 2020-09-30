# Python で ntp server から時刻を取得する (Python 3 版)

[Python で ntp server から時刻を取得する (Python 2 版)](https://qiita.com/c-yan/items/683ba3a8d0c1c13f1bdd) を Python 3 に移植した.
2to3 を使ってみたら、2208988800L の最後の L を取れって言われたので、それを修正して実行してみたら

```
TypeError: a bytes-like object is required, not 'str'
```

って怒られた. しょんぼり.

## テスト

```python
>>> ntp_now('ntp.jst.mfeed.ad.jp')
datetime.datetime(2019, 4, 29, 0, 3, 30)
```

## コード

```python
def ntp_now(server, port = 123):
    from socket import socket, AF_INET, SOCK_DGRAM
    from struct import unpack
    from datetime import datetime
    with socket(AF_INET, SOCK_DGRAM) as s:
        s.sendto(b'\x1b' + 47 * b'\0', (server, port))
        result = s.recvfrom(1024)[0]
    if result:
        return datetime.fromtimestamp(unpack(b'!12I', result)[10] - 2208988800)
    else:
        None
```

b を付けまくって、finally close を with 文で差し替えた. with がデフォルトで使えるのは 2.6 からだから 2.5 時代に書いたのかなあ.
