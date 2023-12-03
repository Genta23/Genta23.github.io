---
title: binary search
layout: default
parent: ライブラリ解説(標準)
grand_parent: AtCoder
---

# Binary Search (二分探索)

ソート済の配列などを対象に探索を行うのが、Binary Searchである。

線形探索を利用すると、計算時間はO(N)となるが、二分探索の場合はO(logN)となる。
## 実装上の注意

- lower_boundやupper_boundを使用してイテレーターを取得する際に、取得した値をそのまま使用することができないことに注意する。

- distanceを使用して、整数型にすると扱いやすい。

```cpp
vector<int> data = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

auto itr = lower_bound(data.begin(), data.end(), 5); // 5以上 戻り値はイテレーター
int d = distance(data.begin(), itr); // 要素5の場所を示す4を返す

itr = upper_bound(data.begin(), data.end(), 5); // 5より大きい
d = distance(data.begin(), itr); // 要素6の場所を示す5を返す
```

### 練習問題

<a href="https://atcoder.jp/contests/abc205/tasks/abc205_d" target="_blank">AtCoder ABC 205 D - Kth Excluded</a>

<a href="https://atcoder.jp/contests/abc321/tasks/abc321_d" target="_blank">AtCoder ABC 321 D - Set Menu</a>

### 参考にしたサイト

<a href="https://pyteyon.hatenablog.com/entry/2019/02/20/194140" target="_blank">競プロ覚書：二分探索，std::lower_bound を使いこなす</a>