---
title: 思ってたんと違う
layout: default
parent: Atcoder (Dec. 2, 2023〜)
has_children: true
---

<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# 思ってたんと違う

うまくいかなかった時の原因をまとめます。チェックリストのようにして高速に確認できるようにします。

## チェックリスト

- [x] <b>デバック用の出力は消したか</b>
- [x] 配列外参照
- [x] 0-based vs 1-based
- [x] 格納する変数のミスはないか 似た変数に間違って格納してないか
- [x] vectorのサイズは適切か(n, m, qの間違いを確認)
- [x] 値渡しと参照渡しを適切に選択できているか
- [x] ループの際に、同じイテレーターを使用していないか
- [x] 頂点数は本当に\\(N\\)ですか?? \\(M\\), \\(2N\\)かも!!