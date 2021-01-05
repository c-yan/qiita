# さくらのレンタルサーバで Python 3 を野良ビルドした (備忘録)

Python 2.7 も EoL で Python 3 に移行したいのに、さくらのレンタルサーバに Python 3 が来る気配は無いので自分でビルドした記録(2020-04-30).

```sh
$ cd /home/xxx/local/src/
$ wget https://www.python.org/ftp/python/3.8.3/Python-3.8.3rc1.tar.xz
$ xz -dc Python-3.8.3rc1.tar.xz | tar xf -
$ cd Python-3.8.3rc1
$ ./configure --prefix=/home/xxx/local/python-3.8.3rc1
$ make
$ make install
$ cd ../../python-3.8.3rc1/bin
$ ./pip3 install --upgrade pip
$ ./pip3 install Genshi
$ ./pip3 install SQLAlchemy
$ ./pip3 install mercurial
$ cd ../../
$ ln -s /home/xxx/local/python-3.8.3rc1 python3
$ cd bin
$ ln -s /home/xxx/local/python3/bin/python3 python3
```

インストールされたバージョンは以下.

```
pip        20.1
Genshi     0.7.3
SQLAlchemy 1.3.16
mercurial  5.3.2
```

新しいものに入れ替えた(2021-01-05).

```sh
$ cd /home/xxx/local/src/
$ wget https://www.python.org/ftp/python/3.8.7/Python-3.8.7.tar.xz
$ xz -dc Python-3.8.7.tar.xz | tar xf -
$ cd Python-3.8.7
$ ./configure --prefix=/home/xxx/local/python-3.8.7
$ make
$ make install
$ cd ../../python-3.8.7/bin
$ ./pip3 install --upgrade pip
$ ./pip3 install Genshi
$ ./pip3 install SQLAlchemy
$ ./pip3 install mercurial
$ cd ../../
$ rm python3
$ ln -s /home/xxx/local/python-3.8.7 python3
```

インストールされたバージョンは以下.

```
pip        20.3.3
Genshi     0.7.5
SQLAlchemy 1.3.22
mercurial  5.6.1
```

関連: [さくらのレンタルサーバで Python をまた野良ビルドした (備忘録)](https://qiita.com/c-yan/items/26aee5cbcba99eced7d3) / [Python 2 から Python 3 に移行したら Mercurial が動かなくなった (備忘録)](https://qiita.com/c-yan/items/b4447233d812868d4ade)
