---
title: グラフの頂点圧縮
layout: default
parent: 考察 解説
grand_parent: Atcoder (Dec. 2, 2023〜)
---

<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# グラフの頂点圧縮

ABC335 E問題などで使用する、グラフの頂点圧縮に関する話。

グラフの頂点を同一視したい場合、Union-Findなどを用いて頂点のグループを管理することによって、グラフの頂点を圧縮することが可能。

その後も頂点にアクセスする際は、find(root)などを利用して、圧縮後の頂点にアクセスするように。

間違って圧縮前の頂点を使用すると変なデータを使用してバグるかも。