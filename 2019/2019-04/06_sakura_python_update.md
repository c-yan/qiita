# さくらのレンタルサーバで Python をまた野良ビルドした (備忘録)

自作アンテナが HTTPS でエラーを出しまくっていて、Python 2.7.6 から 2.7.16 にアップデートしたので、手順を備忘録として.

```sh
$ cd /home/xxx/local/src/
$ wget https://www.python.org/ftp/python/2.7.16/Python-2.7.16.tar.xz
$ xz -dc Python-2.7.16.tar.xz | tar xf -
$ cd Python-2.7.16
$ ./configure --prefix=/home/xxx/local/python-2.7.16
$ make
$ make install
$ cd ../../python-2.7.16/bin
$ wget https://bootstrap.pypa.io/get-pip.py
$ ./python get-pip.py --prefix=/home/xxx/local/python-2.7.16
$ rm get-pip.py
$ ./pip install Genshi
$ ./pip install mercurial
$ ./pip install SQLAlchemy
$ cd ../../
$ rm python
$ ln -s /home/xxx/local/python-2.7.16/ python
```

関連: [さくらのレンタルサーバで Python 3 を野良ビルドした (備忘録)](https://qiita.com/c-yan/items/bff6f857f419756d6512)
