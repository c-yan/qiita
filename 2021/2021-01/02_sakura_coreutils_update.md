# さくらのレンタルサーバで GNU core utilities をまた野良ビルドした (備忘録)

10年前の coreutils を使っていることに気づいたので 8.5 から 8.32 に更新した(2021-01-05).

そもそもなんで入れたのか覚えていない. md5sum コマンドが欲しかっただけとかその程度の理由かもしれない.

```sh
$ cd /home/xxx/local/src/
$ wget https://ftp.gnu.org/gnu/coreutils/coreutils-8.32.tar.xz
$ xz -dc coreutils-8.32.tar.xz | tar xf -
$ cd coreutils-8.32
$ ./configure --prefix=/home/xxx/local/
$ make
$ make install
```
