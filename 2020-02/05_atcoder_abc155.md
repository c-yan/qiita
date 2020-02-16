# AtCoder Beginner Contest 155 参戦記

## ABC155A - Poor

2分で突破. 書くだけ. 前回に引き続き、set での重複判定.

```python
ABC = list(map(int, input().split()))

if len(set(ABC)) == 2:
    print('Yes')
else:
    print('No')
```

## ABC155B - Papers, Please

2分半で突破. 書くだけ.

```python
N = int(input())
A = list(map(int, input().split()))

for a in A:
    if a % 2 == 1:
        continue
    if a % 3 == 0 or a % 5 == 0:
        continue
    print('DENIED')
    exit()
print('APPROVED')
```

## ABC155C - Poll

8分半で突破. 書くだけ……といいつつそれなりに時間がかかったけど(汗). C# 使いが壊滅状態と聞いて、AC した人のコードを眺めると 全員 string 配列のソートに自前の comparer を使っていたので、Mono は string の比較がヤバイのかなと思った.

```python
N = int(input())

d = {}
for _ in range(N):
    S = input()
    if S in d:
        d[S] += 1
    else:
        d[S] = 1

m = max(d.values())
l = []
for k in d:
    v = d[k]
    if v != m:
        continue
    l.append(k)

for s in sorted(l):
    print(s)
```

追記: C# で通している人、みんな自前の comparer を使ってソートしてたけど、標準の StringComparer.Ordinal (これは、実質的には C ランタイムの strcmp 関数の呼び出し)で通るなあ.

```cs
using System;
using System.Collections.Generic;
using System.Linq;

namespace ConsoleApp1
{
    class Program
    {
        static void Main(string[] args)
        {
            var N = int.Parse(Console.ReadLine());

            var d = new Dictionary<string, int>();
            for (var i = 0; i < N; i++)
            {
                var S = Console.ReadLine();
                if (!d.ContainsKey(S)) d[S] = 0;
                d[S]++;
            }

            var m = d.Values.Max();
            var l = new List<string>();
            foreach (var kv in d)
            {
                if (kv.Value != m) continue;
                l.Add(kv.Key);
            }

            l.Sort(StringComparer.Ordinal);
            Console.WriteLine(string.Join("\n", l));
        }
    }
}
```

## ABC155D - Pairs

敗退. Eの方が解いてる人が多いので、Eに行った. にぶたんかなあと思った.

## ABC155E - Payment

敗退. 入力例1, 入力例2 は突破したものの、入力例3が243ではなく、249になってしまって、自分のロジックでうまく行かないパターンを考えていたけど思いつかなかった.
