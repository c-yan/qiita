# AtCoder Beginner Contest 138 D - Ki のテストケースがコンテスト後に増えている件について (修正版)

AtCoder Beginner Contest 138 の終了後も D 問題を解説を見て試行錯誤していたら、AC だったコードが WA になって、after_contest_15, after_contest_16, after_contest_17 が増えていることに気づいた. 解説 PDF にも修正が入っていた.

どうやらテストケースが追加される前は i < j の時に 頂点<sub>i</sub> は 頂点<sub>j</sub> より根に近いという決めつけをしていても通るテストケースになっていたと思われます. 追加されたテストケースはこの決めつけをしていると通らないように思われます. (だから解説 PDF に「根に近い頂点から順に行えば」という追記が入ったと思われる).

なので、追加されたテストケースと同様のテストが出来る入力としては、以下のような 2 と 3 が引っくり返った木で行えそうである.

```
　　①
　／　＼
④　　　③
　　　／
　　②
```

入力例は以下となる.

```
4 3
1 4
1 3
2 3
2 10
1 100
3 1
```

出力例は以下となる.

```
100 111 101 100
```

このテストケースで、追加テストが通らないものは WA になり、追加テストが通るものは AC になるのを確認した.
