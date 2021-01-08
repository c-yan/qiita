# yukicoder contest 277 参戦記

## [A 1329 Square Sqsq](https://yukicoder.me/problems/no/1329)

Python だったら考察しなくても正面から踏み抜けるだろうと踏んで流したら通った(笑). Python を相手にするには 10<sup>10<sup>7</sup></sup> くらい必要なようである.

```python
from math import isqrt

N = int(input())

print(len(str(isqrt(N))))
```

## [B 1330 Multiply or Divide](https://yukicoder.me/problems/no/1330)

コンテスト中は AC したものの、コンテスト後の追加テストによるリジャッジで落とされてしまった.
