# yukicoder contest 252 参戦記

## [A 1076 寿司打](https://yukicoder.me/problems/no/1076)

モンテカルロ法は駄目だったので、真面目に数学しました. 求める期待値を n とすると n = p * (1 + n) + (1 - p) * 0 となるので n = p / (1 - p) となります.

```python
p = float(input())

print(p / (1 - p))
```

## [B 1077 Noelちゃんと星々4](https://yukicoder.me/problems/no/1077)

「広義単調増加 競プロ」で検索すると、POJ-3666 : Making the Grade のコードがでてくるので、広義単調減少列側のコードをコメントアウトして投稿すると解けました(酷).
