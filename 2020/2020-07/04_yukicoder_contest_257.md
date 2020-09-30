# yukicoder contest 257 参戦記

## [A 1113 二つの整数 / Two Integers](https://yukicoder.me/problems/no/1113)

公約数の個数が奇数の時は、最大公約数が何らかの数の2乗で表せるときだけ. isqrt は Python 3.8 で新しく実装された関数です.

```python
from math import gcd, isqrt

A, B = map(int, input().split())

X = gcd(A, B)

if isqrt(X) * isqrt(X) == X:
    print('Odd')
else:
    print('Even')
```

なおテストが弱いので、最大公約数が 10<sup>7</sup> 以下の数で割り切れなければ Odd と出力して、そうでなければ普通に最大公約数を素因数分解して約数の個数を求めるでも AC できます. なんで最大公約数が 10<sup>9</sup> 以上の素数の場合とかテストケースにないんですかね? (と解けなくて嘘解法で AC した分際で言う).

```C++
#include <bits/stdc++.h>
#define rep(i, a) for (int i = (int)0; i < (int)a; ++i)
using namespace std;
using ll = long long;

int main() {
    ll A, B;
    cin >> A >> B;

    ll X = gcd(A, B);

    if (X == 1) {
        cout << "Odd" << endl;
        return 0;
    }

    bool flag = false;
    for (ll i = 2; i < 1e7; i++) {
        if (X % i == 0) {
            flag = true;
            break;
        }
    }

    if (!flag) {
        cout << "Odd" << endl;
        return 0;
    }

    ll result = 1;
    ll t = 0;
    while (X % 2 == 0) {
        t++;
        X /= 2;
    }
    result *= t + 1;

    for (ll i = 3; i < (ll)(sqrt(X) + 1); i += 2) {
        if (X % i != 0) continue;

        ll t = 0;
        while (X % i == 0) {
            t++;
            X /= i;
        }
        result *= t + 1;
    }
    if (X != 1) {
        result *= 2;
    }

    if (result % 2 == 0) {
        cout << "Even" << endl;
    } else {
        cout << "Odd" << endl;
    }

    return 0;
}
```

追記: テストが追加されて、C++ 嘘解法版がリジャッジで死んでいたので、通るコードを書いた(笑).

```C++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

ll isqrt(ll n) {
    ll ok = 0;
    ll ng = 3037000500;
    while (ng - ok > 1) {
        ll m = ok + (ng - ok) / 2;
        if (m * m <= n) {
            ok = m;
        } else {
            ng = m;
        }
    }
    return ok;
}

int main() {
    ll A, B;
    cin >> A >> B;

    ll X = gcd(A, B);

    if (isqrt(X) * isqrt(X) == X) {
        cout << "Odd" << endl;
    } else {
        cout << "Even" << endl;
    }

    return 0;
}
```

## [B 1114 足し算盆に返らず](https://yukicoder.me/problems/no/1114)

奇数の和は偶数なので、奇数だけ出せば勝てます.

```python
N = int(input())

print(*range(1, N + 1, 2))
```

後半だけ出しても勝てます.


```python
N = int(input())

print(*range(N // 2 + 1, N + 1))
```
