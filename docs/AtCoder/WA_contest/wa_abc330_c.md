---
title: repはintで定義している
layout: default
parent: コンテスト中のWA
grand_parent: AtCoder
---

# repはintで定義している
<a href="https://atcoder.jp/contests/abc330/tasks/abc330_c" target="_blank">C - Minimize Abs 2</a>

## 1WA, 2WA
非負整数とみてなぜか1から演算をし、0を除くと言う謎ムーブ

## 3WA
最初は1を使用していたため、
```cpp
for(ll i=1; i<=2000000; i++)
```
と記述していたが、0からできるため、
```cpp
rep(i, 2000000)
```
と書き直した。

エラーを見ているときに完全にオーバーフローの挙動をしていたため、intで落ちている場所を探し続けたがわからなかった。

repでintを使っていることを完全に忘れていた。まさかmain関数外にミスに気づく鍵があるとは... 今度から気をつけよう。