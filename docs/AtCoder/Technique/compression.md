---
title: 座標圧縮
layout: default
parent: 手法解説
grand_parent: AtCoder
---

# 座標圧縮
今回は座標圧縮に関する解説を行います。

自分の過去のコードを見ると、なんでこんなに面倒な書き方したんだろってことがたくさんあるんですけど、一度理解するとかなり簡単です。

## 座標圧縮の目的

<a href="https://atcoder.jp/contests/abc277/tasks/abc277_c" target="_blank">C - Ladder Takahashi</a>

このような問題のときに、ノード番号をそのまま使用しようとすると、探索フラグの管理に大きさが1e9の配列が必要になり、これはメモリ制限に引っ掛かります。

実際に与えられるノードの個数には、そこまで厳しい制約がついていないため、ノード番号が小さい順に数字を割り当ててメモリなどの節約を行おうというのが座標圧縮の目的になります。

## 座標変換vectorの作成

{2, 3, 1, 5, 2, 1, 7, 3}といった数列を、{1, 2, 0, 3, 1, 0, 4, 2}といった数列に変換できれば、目標達成です。

変換の前後で数字を対応づける必要があり、そのために以下の処理を行います。
- 重複した要素を全て削除
- 要素を昇順に並べる

この処理は以下のコードで実現することができます。
```cpp
vector<int> a = {2, 3, 1, 5, 2, 1, 7, 3};

vector<int> b = a; //bを使用して変換vectorを作成する
sort(b.begin(), b.end());
b.erase(unique(b.begin(), b.end()), b.end()); //出力 {1, 2, 3, 5, 7}
```

詳細は重複要素の削除についてというページに記述しています。

実はこのだけで座標変換のためのvectorの作成は完了しています。

## 座標変換vectorの使用方法

ある値が、その問題を解くために使用する値の中で何番目に小さいのか(0始め)を知ることができれば良い。

したがってlower_bound(二分探索)を使用すれば簡単に実現することができる。

lower_boundで帰ってくるイテレータとa.begin()の差を計算すれば、それば圧縮後の座標となる。

```cpp
rep(i, n){
    int v = lower_bound(b.begin(), b.end(), a[i]) - b.begin();
    cout << v << endl; //a[i]の圧縮後の座標を出力する
}
```
## 座標圧縮コードまとめ

```cpp
vector<int> a = {2, 3, 1, 5, 2, 1, 7, 3};

vector<int> b = a; //bを使用して変換vectorを作成する

sort(b.begin(), b.end());
b.erase(unique(b.begin(), b.end()), b.end()); //出力 {1, 2, 3, 5, 7}

rep(i, n){
    int v = lower_bound(b.begin(), b.end(), a[i]) - b.begin();
    cout << v << endl; //a[i]の圧縮後の座標を出力する
}
```