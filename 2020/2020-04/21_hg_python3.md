# Python 2 から Python 3 に移行したら Mercurial が動かなくなった (備忘録)

Mercurial の Python 2 サポートもそう遠くなくドロップされそうなので、Python 3.8 をビルドしたついでに移行してみたら、見事に動かなくなって直した記録.

元々の index.cgi は以下のような感じで、これの python を python3 に書き換えたら 500: Internal Server Error.

```python
#!/home/xxxx/local/bin/python

from mercurial import hgweb
from cgitb import enable
enable()
hgweb.hgweb("/home/xxxx/hg", "xxxx-hg").run()
```

Apache のエラーログを見たところ、`Response header name '<!--' contains invalid characters, aborting request`. 見るからに cgitb が悪さをしていそうなので、ドロップしたら Apache のエラーログにスタックトレースが出力されるようになった. `Mercurial only supports encoded strings` のエラーメッセージでははーんとなり、`hgweb.hgweb("/home/xxxx/hg".encode('utf-8'), "xxxx-hg".encode('utf-8')).run()` に書き換えて動作を確認.

よくよく考えると encode する必要もなく、最終的なソースコードは以下に落ち着いた.

```python
#!/home/xxxx/local/bin/python3

from mercurial.hgweb import hgweb

hgweb(b"/home/xxxx/hg", b"xxxx-hg").run()
```

関連: [さくらのレンタルサーバで Python 3 を野良ビルドした (備忘録)](https://qiita.com/c-yan/items/bff6f857f419756d6512)
