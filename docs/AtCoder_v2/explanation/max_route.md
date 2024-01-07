---
title: 最長経路を求める
layout: default
parent: 考察 解説
grand_parent: Atcoder (Dec. 2, 2023〜)
---

<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# 最長経路を求める

ABC335 E問題などで使用する、ある始点から終点への最長経路を求めるという話。

最短経路を求めるアルゴリズムは有名で、BFSやダイクストラがあるが、最長経路を求めるアルゴリズムに関する話はあまり有名ではないため、まとめておく。

DAGが与えられていて、頂点の強さがわかっていることを前提とする。

