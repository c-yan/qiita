# base64url をデコードする

OpenID Connect に関わるたびに、ID トークンをデコードするために書いてる気がしたので. そもそも Python がパディング無しに対応するオプションを付けてくれればいいのにな…….

```python
import base64

def decode_base64url(s):
  return base64.urlsafe_b64decode(s + b'=' * ((4 - len(s) & 3) & 3))

def encode_base64url(s):
  return base64.urlsafe_b64encode(s).rstrip(b'=')
```
