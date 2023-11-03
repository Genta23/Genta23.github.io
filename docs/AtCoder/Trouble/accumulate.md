---
title: 0の型について
layout: default
parent: トラブルシューティング
grand_parent: AtCoder
---

# なにも考えずに0を使ったらWAした話

## カッコつけてaccumulateを使用したら失敗した

vectorなどの合計値を求めたいときに、いつもは

```cpp
int sum = 0;
rep(i, n) sum += data[i];
```
のように書くのだが、その日はなにを思ったのかaccumulateを使用してスマートに記述したいと思った。

そのため、以下のように記述した。
```cpp
long long sum = accumulate(data.begin(), data.end(), 0);
```

一見問題ないように思えるが、accumulateの戻り値は第3引数の型となる。この場合は0なのでint型が返る。

今回はlong longの計算をしたいと考えているため、第3引数にはlong long型の0を渡す必要がある。

したがって以下のようなコードが正しい。

```cpp
long long sum = accumulate(data.begin(), data.end(), 0LL);
```


