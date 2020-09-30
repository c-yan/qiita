# Python のリストをスタックやキューとして使う

Python には `collections.deque` があり、スタックやキューを使いたい場合それを使えば良いのだが、如何せん `from collections import deque` をググらずに空で書ける気がしないし、popleft も出てくるか怪しいので、リストで代替できるときはしてもいいかなって思った. 時間節約のために.

## スタックとして使う

### Push

特に問題ない.

```python
s.append(v)
```

### Pop

メソッド名もそのままでバッチリ.

```python
v = s.pop()
```

## キューとして使う

### Enqueue

まあ、問題ない.

```python
q.append(v)
```

### Dequeue

どうということもない.

```python
v = q.pop(0)
```
