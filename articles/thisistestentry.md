---
title: "Pythonでフィボナッチ数列を再帰を使って書いてみる"
emoji: ":✌"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["GCP"]
published: false
---

# 題名の通り
フィボナッチ数列とは、ある項の値が、その前、その前の前の項の和で表現される数列です。

```
1,1,2,3,5, ...
```

自然界でも、ひまわりの花の黄金欄などで見ることができます。

## 数式で表してみる
初項を0、xを正の整数としたとき、以下のように表現できます。
`x=0, 1` のとき1と定義されているのは、`-1, -2` の値が存在しないからです。

```
f(0) = 1
f(1) = 1
f(x+2) = f(x+1) + f(x)
```

## 実装（再帰を使わないバージョン）
前の値と、前の前の値を保持しておいて、それらを足し算して値を算出します。
```python
import sys

y = int(sys.argv[1])

prev_prev = 1
prev = 1

for i in range(0, y):
    if i in [0, 1]:
        result = 1
    else:
        result = prev_prev + prev
        prev_prev = prev
        prev = result
    print(i, ':', result)
```

出力
```shell
$ python3 fib0.py 11
0 : 1
1 : 1
2 : 2
3 : 3
4 : 5
5 : 8
6 : 13
7 : 21
8 : 34
9 : 55
10 : 89
```

## 実装（再帰を使ったバージョン）
それでは、再帰を使った場合を書いてみます。
再帰を利用した場合、数式通りの実装のイメージになります。
実装は単純化する一方で、関数が呼ばれる回数が大幅に増えるので、パフォーマンスは劣化します。

```python
import sys

y = int(sys.argv[1])



def fib(x):
    if x in (0, 1):
        return 1
    return fib(x - 1) + fib(x - 2)

for i in range(0, y):
    print(i, ':', fib(i))
```

## 速度の違い

`0-30` の場合でも差がでます。
```
# 再帰を使う
$ time python3 fib0.py 30 > /dev/null

real	0m0.139s
user	0m0.039s
sys	0m0.041s

# 再帰
$ time python3 fib1.py 30 > /dev/null

real	0m0.693s
user	0m0.575s
sys	0m0.044s
```


`0-35` の場合は大幅に差が出てきます。
```
# 再帰を使わない
real	0m0.111s
user	0m0.035s
sys	0m0.037s

# 再帰
real	0m6.195s
user	0m5.861s
sys	0m0.082s
```

40以上を指定すると再帰の方では1分以上帰って来ませんでした。
通常実装では、10000を指定しても、0.5秒ほどで返すことができています。

```
$ time python3 fib0.py 10000 > /dev/null

real	0m0.515s
user	0m0.324s
sys	0m0.073s
```

## まとめ
再帰を使ってフィボナッチ数列を表示するプログラムを実装してみました。
実装は単純化する一方で用途によってはパフォーマンスが問題になるケースがあるため、注意して下さい。
（再帰の実装が悪いわけではなく、空間計算量、メモリを使わないため、適切な問題ではうまく働くケースもあります）
