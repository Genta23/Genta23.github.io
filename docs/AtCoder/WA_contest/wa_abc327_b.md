---
title: powの精度
layout: default
parent: コンテスト中のWA
grand_parent: AtCoder
---

# powの精度
<a href="https://atcoder.jp/contests/abc327/tasks/abc327_b" target="_blank">B - A^A</a>

## 桁数が大きいと精度が担保されなくなる

この問題の解説で十分に理解できるので特に書きませんが、powの精度の部分でWAを出してしまいました。(怪しいけど、ギリいけるかなという気持ちで弾かれた)

コンテスト中はpowlを利用して無理やり通しましたが、このような問題では小数を避けるために整数型で計算するべきです。