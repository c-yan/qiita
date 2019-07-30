# パーセントエンコーディングをデコードする

Python 2 ではサラッとできるのに、Python 3 だと微妙に面倒くささ.

## Python 2 版

```python
>>> s = '%82%A0%82%A2%82%A4%82%A6%82%A8'
>>> import re
>>> pattern = re.compile('%(..)')
>>> t = pattern.sub(lambda m: chr(int(m.group(1), 16)), s)
>>> print t.decode('cp932')
あいうえお
```

## Python 3 版

```python
>>> s = b'%82%A0%82%A2%82%A4%82%A6%82%A8'
>>> import re
>>> pattern = re.compile(b'%(..)')
>>> t = pattern.sub(lambda m: bytes([int(m.group(1), 16)]), s)
>>> print(t.decode('cp932'))
あいうえお
```
