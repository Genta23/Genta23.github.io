---
title: ダイクストラ法
layout: default
parent: AtCoder
has_children: true 
---

# ダイクストラ法
ダイクストラ法は最短経路問題を効率的に解くことができるアルゴリズムです。

startからそれぞれの経路までの最短距離を計算することができます。

このアルゴリズムを使用できる条件として、距離(グラフでいう辺の重み)が非負であることが挙げられます。

## 実装
ライブラリを作成しました。コードに関してはライブラリの項目を参照してください。

## 注意点
ダイクストラは負の辺を含むと無限ループしてTLEになります。

よるあるケースとしては辺の重みをint型で管理して、オーバーフローして負の辺を含み、無限ループが発生しTLEするということです。

## 時間があれば解きたい
<a href="https://atcoder.jp/contests/abc297/tasks/abc297_e" target="_blank">E - Kth Takoyaki Set</a>

## ダイクストラ法問題集