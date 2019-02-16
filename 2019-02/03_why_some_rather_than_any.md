# JavaScript の Array.prototype.some が何故他の言語のように any じゃないか調べてみたけど分からなかった

[JavaScriptのsomeがanyって名前じゃなかったから探しにくかった話](https://qiita.com/RyotaMurohoshi/items/4ca8e8cb566bbc2a23ae)を見てて、ホントなんでなんだろうと思ったので少し調べてみたけど、分からなかったという話.

最初は一瞬 TypeScript の any と被るのが嫌だったのかなあと思ったが、some の出元は Firefox 1.5(2005年11月29日) に搭載された JavaScript 1.6 由来だったので、2010年代の TypeScript はおろか、C# 3.0 (2007年11月19日) と共にリリースされた Linq の Any ですら開発中だった.

当時の開発者向けの記事を [Internet Archive: Wayback Machine](https://archive.org/web/) で漁ってみたが何も書かれていなかったし、Mozilla の Bugzilla で some array で検索しても議論等はでてこなかったので、謎のまま終了.

まあ、これだけ古いと、他の言語に合わせなかったというよりは、他の言語が実装する際に some と any のうち any を選択したため、ぼっちになってしまったということになるのだろう.
