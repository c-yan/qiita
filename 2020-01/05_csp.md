# Content-Security-Policy: default-src 'self' はインライン CSS が機能しない

掲題の通りなので

```
Content-Security-Policy: default-src 'self' 'unsafe-inline'
```

にしましょう. ただし、unsafe-inline は使うべきではないそうです、やったね! (CSP 自体削除したい～)
