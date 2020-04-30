# scp -pr が遅いのを何とかする

大量の小さなファイルを scp -pr をすると遅いのを何とかする.

```sh
$ du -hs 201703
2.3G    201703
$ find . -name '*.gz'|wc -l
32736
```

というわけで、2.3GB、3万個のファイルの転送. 素直に scp -pr すると

```sh
$ time scp -pr 201703 user@203.0.113.4:~/tmp/
Password:

...
...

real    6m46.450s
user    0m28.824s
sys     0m58.958s
```

と 5.8MB/s 程度だった.

これを工夫すると

```sh
$ time tar cf - 201703 | ssh user@203.0.113.4 'cd tmp; tar xf -'
Password:

real    1m50.797s
user    0m16.753s
sys     0m25.793s
```

21.4MB/s に改善.
