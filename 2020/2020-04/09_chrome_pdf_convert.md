# ヘッドレス Chrome で HTML を PDF に変換する

以前にヘッドレス Chrome で HTML を PDF に変換できることを確認したのに、今日試してみたら動かなくなってて、調べたのでメモ.

ググって出てくるのはだいたいこんな感じのコマンドラインだが、これは今は動かない. (どのバージョンから動かなくなったのかは不明だが、Chrome 80.0.3987.163 では動かなかった)

```
"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --headless --disable-gpu --print-to-pdf=C:\foo\bar.pdf file:///C:/foo/bar.html
```

ではどうすればよいか? `--no-sandbox` を付ければ良い. なお、トラブルシューティングするときは `--enable-logging` を付けると良い.

```
"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --headless --disable-gpu --no-sandbox --print-to-pdf=C:\foo\bar.pdf file:///C:/foo/bar.html
```

また、このやり方で PDF を作ると、ヘッダ・フッタが付いて、指定した HTML のファイル名がパス付きで PDF に出力されてしまう. 消したければ以下のような CSS を HTML に入れればいい.

```css
@media print {
    @page {
        margin: 0;
    }
    body {
        margin: 12.7mm;
    }
}
```
