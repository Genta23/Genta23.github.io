---
title: RE+グラフは非連結
layout: default
parent: コンテスト中のWA
grand_parent: AtCoder
---

# グラフは非連結
<a href="https://atcoder.jp/contests/abc327/tasks/abc327_d" target="_blank">B - A^A</a>

## REが発生した

原因はグラフのノード数nと辺の数mが与えられたときに、mと書くつもりがnにしてしまい配列外参照が発生したため。

## 連結グラフの問題ばかり解きすぎた!!

連結グラフだと思い込んだため、ノード1からのBFSで解けると考えたが、実際はこの方法でたどり着けない頂点(非連結のため)が複数存在し、探索不十分によりWA

問題を読んで正確に状況を把握しよう!!