# Python で ntp server から時刻を取得する (Python 2 版)

そういえば、何のために書いたのか思い出せないけど、昔書いたなあと思い出したので.
単純な UDP パケットが飛んでいるだけなんですよね.
まあ、それを基に高度な計算をしているはずなんですが...

これ、Python 2 でしか動かないから Python 3 に移植しよう(笑).

## テスト

```python
>>> ntp_now('ntp.jst.mfeed.ad.jp')
datetime.datetime(2019, 4, 28, 21, 18, 22)
```

## コード

```python
def ntp_now(server, port = 123):
    from socket import socket, AF_INET, SOCK_DGRAM
    from struct import unpack
    from datetime import datetime
    s = socket(AF_INET, SOCK_DGRAM)
    try:
        s.sendto('\x1b' + 47 * '\0', (server, port))
        result = s.recvfrom(1024)[0]
    finally:
        s.close()
    if result:
        return datetime.fromtimestamp(unpack('!12I', result)[10] - 2208988800L)
    else:
        None
```
